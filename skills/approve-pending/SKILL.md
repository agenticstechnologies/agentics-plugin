---
name: approve-pending
description: Find actions waiting for the owner and help them decide. Use when the owner asks "what needs me", "approve pending actions", or after `ask_owner` or `log_action` returns needs_you.
---

# Approve pending actions

Some actions wait under **Needs you** until the owner decides. Only the owner can approve. This agent lists them and explains them. It never approves for the owner.

## Steps

1. Call `my_activity` with `decision: "needs_you"`.
2. For each item, say in one line: what the agent wants to do, the target, any amount, and when it asked (in the owner's time zone, with a label).
3. Send the owner to https://agentics.you/console/needs to choose **Allow** or **Deny**.
4. When they say they are done, call `my_activity` again and report the new decision, with its Check it yourself link.

## Asking the owner

If this agent needs a decision, call `ask_owner` with one clear question that names the exact action. It shows up under **Needs you**.

## Never

- Say an action is approved before a receipt shows `allow`.
- Retry an action that is waiting, or reword it to avoid **Needs you**.
- Treat approval for one action as approval for another action, target, or amount.
