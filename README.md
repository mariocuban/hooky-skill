# Hooky: the AI agent that gets your app users

Paste your app link. Hooky makes short videos for your app, sends them to your TikTok as drafts, and learns
from views and revenue (RevenueCat or Stripe) which posts bring paying users, then makes more of those.

This repo is the Hooky skill and Claude plugin. Use Hooky from the agent you already work in.

- Website: https://usehooky.com
- MCP server: `https://usehooky.com/api/mcp`

## Install

You need a Hooky account first: start at https://usehooky.com with your app link.

**Claude Code (plugin, includes the skill and the MCP server)**
```
/plugin marketplace add mariocuban/hooky-skill
/plugin install hooky@hooky
```
Or just the MCP server:
```
claude mcp add --transport http hooky https://usehooky.com/api/mcp
```
Then run `/mcp` and sign in to Hooky.

**Claude (web and desktop):** Settings → Connectors → Add custom connector → `https://usehooky.com/api/mcp` →
Connect → Allow.

**ChatGPT:** Settings → Apps & Connectors → Advanced settings → Developer mode → Create → paste the address, keep
OAuth → Allow.

**Cursor:** add to `mcp.json`:
```json
{ "mcpServers": { "hooky": { "url": "https://usehooky.com/api/mcp" } } }
```

**OpenClaw, Hermes, Codex:** these use a Hooky key (Settings → Assistants in the Hooky app). Setup lines:
https://usehooky.com/openclaw · https://usehooky.com/hermes · https://usehooky.com/codex

**OpenClaw / ClawHub skill:** `skills/hooky/SKILL.md`.

## What you can ask

- "What should I post for my app this week?"
- "Make a TikTok from the first hook and show it to me."
- "Send it to my TikTok."
- "How did yesterday's posts do? What's bringing users?"

## Tools

`get_status` · `get_hook_ideas` · `start_post` · `ask_hooky` · `list_posts` · `show_post` · `send_post` ·
`hold_post` · `get_results` · `list_clips` · `connect_account`. What each does is in
[`skills/hooky/SKILL.md`](skills/hooky/SKILL.md).

## License

MIT for the files in this repo. Hooky itself is a paid service.
