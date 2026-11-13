---
name: check-post-status
description: Check what state the user's PostFast posts are in (draft, scheduled, published, error) for a date range. Use when the user asks whether a post went out, what is in their calendar, or what drafts exist. Read-only.
license: MIT
compatibility: Needs the PostFast MCP server connected and signed in. Read-only.
---

# Check post status

This skill changes nothing.

## Steps

1. Work out the date range the user means and convert it to two UTC dates. If they gave none, ask, or use the last 7 days and say so.
2. Call `postsListTool` with the start and end dates. Add a channel filter only if the tool supports it and the user named one (call `integrationList` to find the channel).
3. Show each post with its channel, state and the settings the tool returned, as a short table, exactly as returned. Never say a post was published unless its state says so, and do not guess causes the tool did not return.
4. If the user wants to change something on a post that is not published yet, use `postSettingsTool` only for what they ask, and tell them to review it in PostFast. You cannot delete posts.
5. If the tools say sign-in is required, tell the user to authenticate the `postfast` server.
