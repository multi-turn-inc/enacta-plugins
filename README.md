# Enacta plugins

Official [Claude Code](https://claude.com/claude-code) plugins for
[Enacta](https://multi-turn.ai) — memory infrastructure for AI agents:
durable records, observable retrieval, and governed context assembly.

## Install

```
/plugin marketplace add multi-turn-inc/enacta-plugins
/plugin install enacta-memory@enacta
```

Then create an API key at
[app.multi-turn.ai/dashboard/api-keys](https://app.multi-turn.ai/dashboard/api-keys)
and export it where Claude Code runs:

```bash
export ENACTA_API_KEY=your_key
```

## What you get

**enacta-memory** bundles:

- the Enacta MCP server (remote, `https://api.multi-turn.ai/mcp`) with tools
  for adding, searching, and governing memory — including
  `assemble_context`, which returns an action-ready context capsule with
  provenance instead of a raw dump;
- a skill that teaches Claude when to store, how to scope records
  (`user_id` / `agent_id` / `run_id`), and to record whether retrieved
  context actually helped (`record_context_outcome`).

Memory persists across sessions and is inspectable in the
[Enacta dashboard](https://app.multi-turn.ai/dashboard): every record, every
retrieval, with evidence.

## Install as a plain skill (any agent)

The skill also works standalone via the [skills](https://skills.sh) CLI —
Claude Code, Cursor, Codex, and 20+ other agents:

```bash
npx skills add multi-turn-inc/enacta-plugins --skill enacta-memory
```

(The top-level `skills/` directory mirrors
`plugins/enacta-memory/skills/` for skills-CLI discovery; keep both in sync.)

## Works with any MCP client

The same remote server works in any MCP client:

```
URL:    https://api.multi-turn.ai/mcp   (streamable-http)
Header: Authorization: Bearer ENACTA_API_KEY
```

Docs: https://api.multi-turn.ai/docs · Terms: https://multi-turn.ai/terms ·
Privacy: https://multi-turn.ai/privacy

© 2026 Multi-turn Inc.
