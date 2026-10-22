# Changelog

## 1.4.1

- The README lists all 26 tools a sign-in connection gets from `https://agentics.you/mcp`, matching the live `tools/list`: memory, receipts and actions, owner and limits, handoffs, and subscriptions. It adds `record_action_batch`, `get_agent_history`, `update_memory`, `forget_memory`, `subscribe`, `unsubscribe`, and `list_subscriptions`, and says an agent key lists 20 (no handoff tools).
- Each tool is marked Read, Write, or Destructive as the server annotates it. `my_rules` and `unsubscribe` are Destructive.
- Replaces "None of these tools moves money or deletes data" with what forget, update, and unsubscribe actually do.
- No change to the MCP server entry, skills, or sign-in.

## 1.4.0

- Claude Code: `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`, so this repository installs with `claude plugin marketplace add agenticstechnologies/agentics-plugin` and `claude plugin install agentics@agentics`.
- Two memory skills: `save-memory` (calls `remember`) and `recall-memory` (calls `recall`, or `get_context` for a deeper search).
- The README leads with shared memory, lists the memory and hub tools and the current scopes, and explains agent keys (`AGENTICS_AGENT_KEY`) versus the console key.
- CI: `plugin.yml` checks the manifests agree and installs the plugin with the Claude Code CLI (install smoke).

## 1.3.0

First public release.

- Connects to the hosted Agentics MCP server at `https://agentics.you/mcp`. Sign-in is OAuth 2.1 with PKCE. There is no API key to set.
- Skills: `show-receipts`, `log-what-my-agent-did`, and `approve-pending`.
- The README lists the seven tools a new connection sees, each marked read or write.
- Adds privacy, terms, and support links.
- Uses `assets/logo.svg` as the plugin logo.

Earlier versions were private.
