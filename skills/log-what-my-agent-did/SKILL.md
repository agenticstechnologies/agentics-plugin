---
name: log-what-my-agent-did
description: Log what this agent did and report the outcome. Use when the owner asks to record an action, asks what the agent did, or an action needs a durable outcome.
---

# Log what my agent did

Use the Agentics action log for an action this agent took or is about to take.

## Steps

1. Gather the action, the target, any dollar amount, and the result. Do not invent fields the owner or the work did not provide.
2. Before any **send**, **delete**, **sign**, **publish**, or **share** action, call `ask_owner` with one clear question naming the exact action. Do not perform the action until the owner approves it. A general instruction does not approve a new action, target, or amount.
3. For an action that is about to happen, call `log_action` using the server's current schema. For something this agent already finished, call `record_action`. Its receipt is marked self-reported.
4. When the result is known, call `report_outcome` with the result and the receipt id returned in step 3.
5. Tell the owner what was recorded. Include only links and values the tools returned.

## Never

- Skip `ask_owner` for send, delete, sign, publish, or share actions.
- Call `report_outcome` before there is a receipt to attach it to.
- Claim an action succeeded when the tools report failure.
- Invent a receipt, hash, link, or outcome.
- Put secrets, access tokens, or personal data in an action, target, or summary.
