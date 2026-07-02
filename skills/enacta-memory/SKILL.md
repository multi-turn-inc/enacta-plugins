---
name: enacta-memory
description: Store and retrieve long-term memory with Enacta. Use when the user asks to remember or recall something across sessions, when starting a task that would benefit from prior project context, or when a durable decision, preference, or incident note is worth keeping. Requires ENACTA_API_KEY.
---

# Enacta memory

Enacta gives agents durable, searchable memory with evidence you can inspect.
This plugin bundles the Enacta MCP server (named `enacta`); its tools are
available once `ENACTA_API_KEY` is set.

## Setup (once)

1. Create an API key at https://app.multi-turn.ai/dashboard/api-keys
   (free Hobby plan works).
2. Export it in the environment where Claude Code runs:
   `export ENACTA_API_KEY=...`

If the `enacta` MCP tools are not listed, the key is missing or invalid —
tell the user rather than silently continuing without memory.

## The core loop

- **Before starting substantive work**, call `assemble_context` with a short
  query describing the task. It returns a ready-to-use context capsule with
  `use_now` items and provenance. Prefer it over raw search when preparing a
  turn.
- **When a durable fact appears** — a decision and its why, a user
  preference, an environment constraint, an incident postmortem — call
  `add_memory` with the fact as a short self-contained statement.
- **Verify writes you depend on**: `search_memories` with a related query
  should surface what you just stored.
- **After using retrieved context**, call `record_context_outcome` to record
  whether it actually helped. This feedback improves later retrieval.

## Scoping rules

Always scope records so retrieval stays precise:

- `user_id` — one stable id per human (e.g. their handle).
- `agent_id` — set when the memory belongs to an agent role rather than a person.
- `run_id` — set for session-scoped working state you may want to expire.

## What to store / what not to store

Store: decisions with rationale, durable preferences, project conventions,
recurring pitfalls, incident notes. One fact per record, written so it makes
sense a month later.

Do not store: secrets or credentials, large file dumps, transient state that
expires with the session, or anything the user asked to keep private.

## REST fallback

The same API works over plain HTTP when MCP is unavailable:

```bash
# add
curl https://api.multi-turn.ai/v1/memories/ \
  -H "Authorization: Token $ENACTA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"messages":[{"role":"user","content":"We deploy from main only; hotfixes get a release branch."}],"user_id":"quickstart-user"}'

# search
curl https://api.multi-turn.ai/v3/memories/search/ \
  -H "Authorization: Token $ENACTA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query":"deploy policy","filters":{"user_id":"quickstart-user"}}'
```

Full API reference: https://api.multi-turn.ai/docs
