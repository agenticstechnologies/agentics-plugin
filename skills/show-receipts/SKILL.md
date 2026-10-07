---
name: show-receipts
description: Show this agent's receipts, recent actions, and spend, each with a Check it yourself link. Use when the owner asks "show my receipts", "what did my agent do", "what did I spend?", "what was denied", or wants proof of an action.
---

# Show my receipts

Every action recorded through Agentics leaves a receipt. Anyone with the link can check one at https://agentics.you/verify/.

## Steps

1. Pick the tool:
   - All receipts for this agent: `receipts` (up to 25).
   - Filtered actions for this agent: `my_activity` with `decision` (`allow`, `deny`, `needs_you`), `action_type`, `from`, `to`, or `limit` (up to 50).
   - This agent's record and the receipts behind it: `my_record`.
2. Show a short table: time, action, amount, decision, provenance, and the Check it yourself link. Put times in the owner's time zone with a label.
3. For "what did I spend?", add up only the amounts on the receipts the tool returned. Say how many receipts that covers and the time range. If the tool hit its limit, say the total may be incomplete.
4. Explain provenance in one line when it appears:
   - `verified_by_agentics`: Agentics saw the action and recorded it.
   - `self_reported`: the agent reported it. Agentics did not see it happen.
   - `imported`: it came from another system.
5. Point out anything denied or waiting under **Needs you**.

## Never

- Invent a receipt, a hash, an amount, or a link. Only show what the tools return.
- Call a `self_reported` receipt verified.
- Present a partial total as the full total.
