<!--
TODO(copy): restore the approved tile once the hub tools (whoami, list_peers, send_task,
inbox, submit_result, get_result) are live on https://agentics.you/mcp:
  Tile: "Connect your agents, delegate anything, track everything."
  Second line: "Your Muse hands work to your Grok Bot."
Until then, use "Connect your agents. Track everything they do." here and in
.cursor-plugin/plugin.json "description".
-->

# Agentics

**Connect your agents. Track everything they do.**

Agentics keeps receipts, spend, and approvals for your AI agents in one place. This plugin connects Grok Bot or Cursor to the hosted Agentics [MCP](https://modelcontextprotocol.io/) server at `https://agentics.you/mcp`.

Agentics never moves your money. Nothing spends, sends, or publishes without your approval.

## Install

From the marketplace: open **Marketplace** in Grok Bot (or **Customize** in Cursor), find **Agentics**, and choose **Add** or **Install**.

Then connect:

1. Choose **Connect**. Agentics opens in your browser.
2. Sign in to Agentics, or create an account. Pick the agent this connection acts as, then choose **Allow**.
3. Ask your agent: "What did I spend?"

Without the plugin, add the server by hand in any MCP client: "Add a custom MCP server called agentics at https://agentics.you/mcp".

## Connection and sign-in

```json
{
  "mcpServers": {
    "agentics": {
      "type": "http",
      "url": "https://agentics.you/mcp",
      "placement": "server"
    }
  }
}
```

- **Endpoint:** `https://agentics.you/mcp` (Streamable HTTP). This is the only network endpoint the plugin uses.
- **Sign-in:** OAuth 2.1 with dynamic client registration and PKCE (`S256`). There is no API key, client ID, or variable to set, and the plugin stores no credentials.
- **Scopes:** `mcp:tools`, `receipts:read`, `owner:ask`, `actions:record`.
- **Disconnect:** revoke the connection any time in the Agentics console.

The plugin has no hooks, scripts, or local code. It is an MCP server entry plus three skills.

## Tools

A new connection sees seven tools. The hosted server is the source of truth for tool names and schemas.

| Tool | Type | What it does |
| --- | --- | --- |
| `receipts` | Read | Lists up to 25 of this agent's receipts, each with a Check it yourself link |
| `my_activity` | Read | This agent's recent actions, filterable by decision, action type, and time (up to 50) |
| `my_record` | Read | This agent's record and the receipts behind it |
| `ask_owner` | Write | Asks you a question. It shows up under **Needs you** |
| `log_action` | Write | Records an action. Send, delete, sign, publish, and share wait for you unless a rule you set allows them |
| `record_action` | Write | Records something the agent already did. The receipt is marked self-reported, because Agentics did not see it happen |
| `report_outcome` | Write | Adds a result and a dollar impact to an existing receipt, as a new linked receipt |

None of these tools moves money or deletes data.

## Skills

| Skill | Use it to |
| --- | --- |
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
