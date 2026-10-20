---
name: recall-memory
description: Recall what Agentics shared memory holds about a person, project, preference or past decision before answering. Use when the owner asks "what do you remember about", "what did we decide", "do you know my preference for", or when an answer depends on context from earlier sessions or other agents.
---

# Recall from memory

## Steps

1. Turn the question into a short search query that names the subject, e.g. "Dana seat preference flights". The prefixes `tag:`, `task:`, `entity:` and `agent:` match exactly, e.g. `entity:Dana`.
2. Call `recall` with `query`. Add `category` (e.g. `["preference"]`) or `limit` when it narrows the answer. For a deeper search that also follows causes and effects, use `get_context` with `query` and `traverse` true.

   If neither tool is listed, the memory tools are off for this connection. Say so and answer without memory.
3. Use only what came back. Quote or paraphrase the memories that answer the question, and say where each came from when the result names an agent or a receipt.
4. If nothing relevant came back, say so plainly and answer without it, or ask the owner. Offer to save the answer with the `save-memory` skill.

## Never

- Invent a memory, a receipt or a link. Only use what the tool returned.
- Treat an old memory as current when a newer one contradicts it. Prefer the newer one and mention the change.
