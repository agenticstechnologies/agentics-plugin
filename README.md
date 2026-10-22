# Agentics

**Shared memory for your agents, with a receipt for every write.**

Agentics gives your AI agents one memory they share. What one agent saves, the others can recall. Every write leaves a receipt you can check, and receipts, spend and approvals stay in one place. This plugin connects Claude Code, Grok Bot or Cursor to the hosted Agentics [MCP](https://modelcontextprotocol.io/) server at `https://agentics.you/mcp`.

Agentics never moves your money. Nothing spends, sends, or publishes without your approval.

## Install

### Claude Code

```bash
claude plugin marketplace add agenticstechnologies/agentics-plugin
claude plugin install agentics@agentics
```

Or, inside Claude Code (2.1.275 or later):

```
/plugin install agentics --marketplace agenticstechnologies/agentics-plugin
```

Then run `/mcp`, pick **agentics**, and sign in. Try "Remember that I prefer aisle seats", then, in a new session, "What do you remember about my seat preference?"

### Grok Bot and Cursor

Open **Marketplace** in Grok Bot (or **Customize** in Cursor), find **Agentics**, and choose **Add** or **Install**. Then:

1. Choose **Connect**. Agentics opens in your browser.
2. Sign in to Agentics, or create an account. Pick the agent this connection acts as, then choose **Allow**.
3. Ask your agent: "What do you remember about me?"

### Any other MCP client

"Add a custom MCP server called agentics at https://agentics.you/mcp".

## Connection and sign-in

```json
{
  "mcpServers": {
    "agentics": {
      "type": "http",
      "url": "https://agentics.you/mcp"
    }
  }
}
```

- **Endpoint:** `https://agentics.you/mcp` (Streamable HTTP). This is the only network endpoint the plugin uses.
- **Sign-in:** OAuth 2.1 with dynamic client registration and PKCE (`S256`). There is nothing to set, and the plugin stores no credentials.
- **Scopes:** `mcp:tools`, `receipts:read`, `owner:ask`, `actions:record`, `rules:read`, `hub:tasks`.
- **Agent keys:** a client that cannot sign in can send an agent key as `Authorization: Bearer`. Set it as `AGENTICS_AGENT_KEY`. The console key (`ak_…`, `AGENTICS_API_KEY`) is for the REST API and SDKs, and `/mcp` does not accept it. See the [quickstart](https://agentics.you/docs/quickstart/).
- **Disconnect:** revoke the connection any time in the Agentics console.

The plugin has no hooks, scripts, or local code. It is an MCP server entry plus five skills.

## Tools

A connection made through sign-in lists 26 tools. A connection made with an agent key lists 20: the key does not carry the `hub:tasks` scope, so the six handoff tools are not listed. The hosted server is the source of truth for tool names and schemas: `tools/list` shows what your connection has. The full reference is at https://agentics.you/docs/mcp/tools/.

**Read** tools are marked read-only by the server. **Write** tools add or change something in Agentics. **Destructive** tools are marked `destructiveHint: true` by the server, so clients ask before running them.

### Memory

| Tool | Type | What it does |
| --- | --- | --- |
| `remember` | Write | Saves something worth knowing later as a memory your agents can recall, with a receipt that proves when it was saved |
| `recall` | Read | Searches your memories and returns the best matches, each with a score and a link to check it |
| `get_context` | Read | Returns what your agents already know about a query, with reasoning, outcome, and a link to check each |
| `find_related` | Read | Finds memories connected to one memory: causes and effects, the same task, shared entities or tags |
| `get_agent_history` | Read | Lists what one agent recorded, newest first. Defaults to this agent |
| `update_memory` | Write | Corrects a memory. Saves the correction as a new memory, marks the old one replaced, and writes a signed receipt of the change |
| `forget_memory` | Write | Makes one memory forgotten, so `recall` and `get_context` stop returning it. A signed receipt records the forget. Pause all and approval rules apply |
| `record_outcome` | Write | Records what really happened to one memory, as a new outcome receipt. The original receipt is unchanged |

### Receipts and actions

| Tool | Type | What it does |
| --- | --- | --- |
| `log_action` | Write | Call before an action. Writes a receipt and returns allow, deny, or needs_you. Send, delete, sign, publish, and share wait for you unless a rule you set allows them |
| `record_action` | Write | Records an action the agent already took, with optional reasoning and lessons. The receipt is marked self-reported, because Agentics did not see it happen |
| `record_action_batch` | Write | Records up to 50 actions in one request. Each succeeds or fails on its own |
| `report_outcome` | Write | Adds success or failure, and an optional dollar impact, to one receipt, as a new linked receipt |
| `receipts` | Read | Lists this agent's newest receipts with verdicts, dollars, and Check it yourself links |
| `my_activity` | Read | Lists this agent's recorded actions, newest first, filterable by decision, action type, provenance, and time |
| `my_record` | Read | Reads this agent's record and its ten newest receipts |

### Owner and limits

| Tool | Type | What it does |
| --- | --- | --- |
| `ask_owner` | Write | Asks you a question. It shows up under **Needs you** and returns only that the agent is waiting |
| `my_rules` | Destructive | Reads your rules for this agent. Called with `pause`, it pauses every agent (Pause all). Only you can resume |

### Handoffs between your agents (sign-in connections)

| Tool | Type | What it does |
| --- | --- | --- |
| `whoami` | Read | Shows which agent, harness, and owner this connection belongs to |
| `list_peers` | Read | Lists your other agents and whether this one may send them work |
| `send_task` | Write | Hands work to one connected agent, or up to ten |
| `inbox` | Write | Lists work waiting for this agent, and takes a task when the agent is ready to do it |
| `submit_result` | Write | Finishes a task the agent took, with a short summary and links |
| `get_result` | Write | Reads status, lineage, and spend for a task the agent sent or was given, and can reject a result |

### Subscriptions

| Tool | Type | What it does |
| --- | --- | --- |
| `subscribe` | Write | Sends matching new memories to an https webhook. Up to 50 per agent |
| `unsubscribe` | Destructive | Stops one subscription by its id |
| `list_subscriptions` | Read | Lists this agent's subscriptions and when each last matched |

None of these tools moves money. No tool deletes a receipt: `forget_memory` stops a memory coming back from recall, `update_memory` keeps the old text as history, and `unsubscribe` stops a webhook.

## Skills

| Skill | Use it to |
| --- | --- |
| `save-memory` | Save a fact, decision or preference to shared memory |
| `recall-memory` | Recall what memory holds before answering |
| `show-receipts` | Show receipts, recent actions, and spend |
| `log-what-my-agent-did` | Log an action, then report its outcome |
| `approve-pending` | Find what needs you and decide in Agentics |

## Approvals

The agent cannot write a rule or approve its own request. You set rules and decide **Needs you** items at https://agentics.you/console/.

The skills ask you through `ask_owner` before any **send**, **delete**, **sign**, **publish**, or **share** action. Each approval covers one action. If you don't approve, the agent stops.

## Privacy and support

- Privacy policy: https://agentics.you/privacy
- Terms: https://agentics.you/terms
- Support: https://agentics.you/help (or hello@agentics.you)
- Security reports: security@agentics.you
- Docs: https://agentics.you/docs/mcp/
- Check a receipt: https://agentics.you/verify/

## License

[MIT](LICENSE) © 2026 Agentics Technologies LLC
