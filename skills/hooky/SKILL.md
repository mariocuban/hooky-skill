---
name: hooky
description: Get users for an app from TikTok with Hooky, the AI distribution agent. Use when the user wants users, downloads or revenue for their app from short videos (TikTok, Reels, Shorts), asks what to post, wants hook ideas, wants a TikTok post made or sent, or asks how their posts are doing.
---

# Hooky: the AI agent that gets your app users

Hooky makes short videos and slideshows for an app, sends them to the user's TikTok as drafts, and learns
from views and revenue (RevenueCat or Stripe) which posts bring paying users, then makes more of those.

You talk to it through its MCP server: `https://usehooky.com/api/mcp`

## Setup

The user needs a Hooky account (start at https://usehooky.com with their app link). Then add the MCP server:

| Agent | How |
|---|---|
| Claude (web, desktop) | Settings → Connectors → Add custom connector → paste the address → Connect → Allow |
| ChatGPT | Settings → Apps & Connectors → Advanced → Developer mode → Create → paste the address, OAuth → Allow |
| Claude Code | `claude mcp add --transport http hooky https://usehooky.com/api/mcp`, then `/mcp` to sign in |
| Cursor | Add `{"mcpServers":{"hooky":{"url":"https://usehooky.com/api/mcp"}}}` to mcp.json; Cursor opens Hooky to sign in |
| OpenClaw, Hermes, Codex | Use a Hooky key from Settings → Assistants in the Hooky app (setup lines at https://usehooky.com/openclaw, /hermes, /codex) |

Agents that sign in open Hooky's page and the user presses Allow. Never ask the user for a key or password in
the chat.

## Tools

| Tool | Use it for |
|---|---|
| `get_status` | Start of every conversation: the app, plan, posts left this month, whether TikTok and revenue are connected, posts in progress and drafts waiting |
| `get_hook_ideas` | "What should I post?": four hooks with the reason each should work, based on the account's own results |
| `start_post` | Make a post from a hook in the background (`format`: `video` or `slides`); `list_posts` shows it when ready |
| `ask_hooky` | Anything that needs judgement: make or change a post, what is working, what to post next. Pass the user's own words |
| `list_posts` | The latest posts and where each one stands |
| `show_post` | Show one post in the chat; call it after `ask_hooky` made or changed a post |
| `send_post` | Send a finished post to the user's TikTok inbox as a draft, **only when the user asks** |
| `hold_post` | Stop a post from being sent in the morning, or release it |
| `get_results` | Views, likes, comments, shares per post, the last 7 days against the 7 before, and revenue when connected |
| `list_clips` | The clips Hooky can use for the user's app |
| `connect_account` | When TikTok is not connected and the user wants to send, or revenue is not connected and they ask about money |

## How to run it

1. `get_status`. If TikTok is not connected, call `connect_account` with `tiktok`.
2. `get_hook_ideas`, then let the user pick one.
3. `start_post` with that hook (or `ask_hooky` if you can wait a few minutes), then `show_post`.
4. When the user says to send it: `send_post`. The draft lands in their TikTok inbox; they add a trending sound
   and publish.
5. Next day: `get_results`, then `get_hook_ideas` again. Hooky doubles down on what is working.

## Rules

- Only say a post was sent if `send_post` returned `sent: true`.
- State only the numbers `get_results` gives. Revenue is for the whole app and is never credited to one post.
- Making a post can take several minutes; if a call is cut off, the post is still being made and `list_posts`
  shows it.
- If the plan has ended, Hooky says so and gives the plan page; pass that on.
