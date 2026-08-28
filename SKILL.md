---
name: bluecolumn-memory
description: Store and recall persistent memory for AI agents using BlueColumn. Agents can save facts, decisions, preferences, promises, and action items, then query them later with natural language. Installable via `npx skills add bluecolumnconsulting-lgtm/bluecolumn-agent-skill` or copy into any agent's skill directory.
version: 0.1.0
---

# BlueColumn Memory (agent-ready)

BlueColumn gives AI agents **persistent, searchable memory**. Store anything (facts, decisions, preferences, promises, action items, transcripts), then recall it later with natural-language queries — with citations back to the source.

This is the agent-facing skill. It works with any assistant that can call HTTP APIs (OpenClaw, Claude Code, Cursor, or raw HTTP).

## Setup

1. Get an API key at [bluecolumn.ai](https://bluecolumn.ai) (free tier: 100 writes, 100 reads, 30 audio minutes/month — no card required).
2. Store the key wherever your agent keeps secrets (env var, secret store, or a private local notes file). Keys look like `bc_live_...`.

## Endpoints

Base URL: `https://xkjkwqbfvkswwdmbtndo.supabase.co/functions/v1`

### 1. RECALL — query memory

```
POST /agent-recall
Content-Type: application/json
Authorization: Bearer <api-key>
```

Body:
```json
{
  "q": "What did the user decide about the billing design?"
}
```

Returns: AI-synthesized `answer` with source citations (session ids, timestamps, relevance scores).

### 2. REMEMBER — store memory

```
POST /agent-remember
Content-Type: application/json
Authorization: Bearer <api-key>
```

Body:
```json
{
  "text": "Full raw content to store (transcript, doc, note...)",
  "title": "Topic label, e.g. Meeting - Q3 planning - 2026-08-27"
}
```

Returns: `session_id`, chunk count, auto-extracted `summary`, `action_items`, `key_topics`.

### 3. NOTE — quick observation (lightweight)

```
POST /agent-note
Authorization: Bearer <api-key>
```

Body:
```json
{
  "text": "User prefers concise replies with concrete next steps",
  "tags": ["preference"]
}
```

Short vector note for preferences and quick observations (min 5 chars). Optional tags.

## Field-name gotchas

- `/agent-remember` takes `text`, not `content`
- `/agent-recall` takes `q`, not `query`
- `/agent-note` takes `text`, not `note`

## Usage pattern (skill-style)

- **Store a transcript:** `REMEMBER title="YouTube - <title> <date>" text=<full text>`
- **Ask a question:** `RECALL q="What are the key action items from the last planning session?"`
- **Save a decision:** `NOTE text="User confirmed: deploy on Fridays only after smoke tests pass" tags=["decision"]`

## Why agents should use this

- Memory persists across sessions — no context loss.
- Synthesized answers with citations — verify claims back to source.
- Auto-extracted summaries and action items on every write.
- Simple REST surface — works from any language or agent framework.

## Related

- MCP server (Claude Desktop / Claude Code / Cursor): `npx bluecolumn-mcp` — [github.com/bluecolumnconsulting-lgtm/bluecolumn-mcp](https://github.com/bluecolumnconsulting-lgtm/bluecolumn-mcp)
- Docs: [bluecolumn.ai/docs](https://bluecolumn.ai/docs)
- ClawHub: the `bluecolumn-memory` skill
