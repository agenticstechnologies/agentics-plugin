---
name: save-memory
description: Save something worth remembering to Agentics shared memory, so this agent and the owner's other agents can recall it later. Use when the owner says "remember this", "save that", "note that I prefer", or states a durable fact, decision or preference about their work.
---

# Save a memory

Agentics keeps memories for the owner's agents. Each write leaves a receipt.

## Steps

1. Decide what to save. Write it as one self-contained statement a different agent could use without this conversation, e.g. "Dana prefers aisle seats on flights over two hours." Include who and what. Leave out secrets.
2. Call `remember`:
   - `content`: the statement.
   - `category` when it fits: `preference`, `decision`, `fact`, `outcome`, `failure` or `reflection`. A newer preference, decision or fact replaces an older one on the same subject.
   - `tags` and `entities` to find it by, e.g. tags `["travel"]`, entities `["Dana"]`.
   - `reasoning` when the owner said why it matters.

   If `remember` is not listed, the memory tools are off for this connection. Use `record_action` with `action` `memory.note` and `summary` set to the statement, and tell the owner the memory tools are off.
3. Confirm in one line what was saved, with the receipt link or `receipt_hash` the tool returned.

## Never

- Save credentials, card numbers or other secrets. If the owner asks, say Agentics memory is not a secret store.
- Save something the owner did not say or approve. When unsure, ask before saving.
- Claim it was saved if the tool returned an error. Say what the error was.
