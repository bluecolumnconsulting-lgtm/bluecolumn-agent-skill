# bluecolumn-agent-skill

Agent-ready skill for [BlueColumn](https://bluecolumn.ai) — persistent, searchable memory for AI agents.

## Install

### OpenClaw / skills CLI
```bash
npx skills add bluecolumnconsulting-lgtm/bluecolumn-agent-skill
```

### Manual
Copy [`SKILL.md`](SKILL.md) into your agent's skill directory (e.g. `~/.openclaw/workspace/skills/bluecolumn-memory/`).

## What it does

Gives your agent three REST calls:

| Endpoint | Purpose |
|---|---|
| `POST /agent-remember` | Store text, docs, or audio URLs — auto-extracts summary + action items |
| `POST /agent-recall` | Natural-language query → synthesized answer with source citations |
| `POST /agent-note` | Lightweight observation writes (preferences, decisions) |

Free tier: 100 writes + 100 reads + 30 audio minutes/month, no credit card. Get a key at [bluecolumn.ai](https://bluecolumn.ai).

## MCP alternative

If your agent speaks MCP (Claude Desktop, Claude Code, Cursor):

```bash
npx bluecolumn-mcp --api-key=bc_live_xxxxxxxxxxxx
```

## Example

```bash
# Store
curl -X POST https://xkjkwqbfvkswwdmbtndo.supabase.co/functions/v1/agent-remember \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer bc_live_YOUR_KEY" \
  -d '{"text": "User is building a CRM agent; prefers TypeScript", "title": "User profile"}'

# Recall
curl -X POST https://xkjkwqbfvkswwdmbtndo.supabase.co/functions/v1/agent-recall \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer bc_live_YOUR_KEY" \
  -d '{"q": "What is the user building?"}'
```

## License

MIT
