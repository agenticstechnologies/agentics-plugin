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

The hosted server is the source of truth for tool names and schemas: `tools/list` shows what your connection has. The full reference is at https://agentics.you/docs/mcp/tools/.

| Tool | Type | What it does |
| --- | --- | --- |
| `remember` | Write | Saves a memory, with a receipt |
| `recall` | Read | Finds memories for a query |
| `get_context` | Read | Deeper recall that can follow causes and effects |
| `find_related` | Read | Memories linked to a receipt |
| `record_outcome` | Write | Adds what really happened to a memory |
| `receipts` | Read | Lists up to 25 of this agent's receipts, each with a Check it yourself link |
| `my_activity` | Read | This agent's recent actions, filterable by decision, action type, and time (up to 50) |
| `my_record` | Read | This agent's record and the receipts behind it |
| `my_rules` | Write | Reads your rules, and can Pause all. Only you can resume |
| `ask_owner` | Write | Asks you a question. It shows up under **Needs you** |
| `log_action` | Write | Records an action. Send, delete, sign, publish, and share wait for you unless a rule you set allows them |
| `record_action` | Write | Records something the agent already did. The receipt is marked self-reported, because Agentics did not see it happen |
| `report_outcome` | Write | Adds a result and a dollar impact to an existing receipt, as a new linked receipt |
| `whoami`, `list_peers`, `send_task`, `inbox`, `submit_result`, `get_result` | Read and write | Hand work between your connected agents (signed-in connections) |

None of these tools moves money or deletes data.

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
