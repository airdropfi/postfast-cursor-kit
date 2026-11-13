# PostFast for Cursor and Grok Build

> PostFast lets an AI assistant draft YouTube and TikTok video posts in your PostFast account; you review and press Post in PostFast.

## PostFast at a glance

- **What it is:** PostFast lets an AI assistant draft YouTube and TikTok video posts in your PostFast account; you review and press Post in PostFast.
- **Primary user:** An AI assistant (Claude, Cursor, Gemini CLI or any MCP client) lists your connected channels, prepares media and creates drafts. You open each draft in PostFast, review it and press Post now or Add to calendar.
- **Channels live today:** YouTube (Shorts and videos: title, description, privacy public/unlisted/private, made-for-kids, thumbnail), TikTok (caption, who can view, comments, duet, stitch, commercial-content disclosure; Direct Post or inbox upload), Bluesky (handle and app password), WordPress, DEV and Hashnode. Other networks are not available yet.
- **What the AI can do:** Eight tools: `integrationList`, `groupList`, `integrationSchema`, `triggerTool`, `createDraftPostTool`, `postsListTool`, `postSettingsTool` and `uploadFromUrlTool`. It only creates drafts: it never publishes or schedules, and it cannot delete posts.
- **Human oversight:** Nothing is published until you open the draft in PostFast and press Post now or Add to calendar. TikTok privacy, comment, duet, stitch and commercial-content disclosure are choices you make in the PostFast composer. You can revoke the connection in PostFast.
- **Cost:** You can create a free PostFast account. Plan terms are in the [terms](https://postfast.sh/terms).
- **MCP:** https://postfast.sh/api/mcp (Streamable HTTP, stateless). Sign in with OAuth 2.1 (authorization code, PKCE S256, dynamic client registration; you sign in to PostFast and approve) or send a PostFast API key as `Authorization: Bearer <key>`. Legacy equivalent: https://postfast.sh/api/mcp-oauth-dynamic.
- **Source:** AGPL-3.0, offered on request: [email support@postfast.sh](mailto:support@postfast.sh?subject=Source%20code%20request).

## Connect in 60 seconds

1. Create an API key in PostFast Settings, in the Public API section, and export it: `export POSTFAST_API_KEY=YOUR_API_KEY`, then restart Cursor.
2. Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "postfast": {
      "url": "https://postfast.sh/api/mcp",
      "headers": { "Authorization": "Bearer ${env:POSTFAST_API_KEY}" }
    }
  }
}
```

You need a PostFast account (free to create) with a channel connected.

## Example prompts

You ask; the assistant prepares a draft; you review and press Post in PostFast.

- `Use PostFast to list my connected channels. Do not create anything.`
- `Use PostFast to draft a YouTube Short from https://example.com/clip.mp4 with the title "Launch day recap". Ask me for privacy and made-for-kids before you create the draft.`
- `Use PostFast to draft this TikTok video on my TikTok channel. Ask me for who can view it, comments, duet, stitch and commercial-content disclosure first.`
- `Use PostFast to list my posts from 2026-11-01 to 2026-11-15 and tell me each state exactly as returned.`

## Human oversight: what you can see, approve and revoke

- **Drafts only.** `createDraftPostTool` creates a DRAFT. It never publishes or schedules. The post goes out only when you open it in PostFast and press Post now or Add to calendar.
- **No deletion.** The AI cannot delete posts.
- **Your choices on TikTok.** Privacy, comments, duet, stitch and commercial-content disclosure are shown to you in the PostFast composer, and TikTok publishing needs your explicit choices there.
- **Approve or refuse a connection.** With OAuth you sign in to PostFast and approve the connection.
- **Revoke access.** You can revoke the OAuth connection or the API key in PostFast.

## Links

- Product: https://postfast.sh
- MCP page with setup (markdown: https://postfast.sh/mcp.md): https://postfast.sh/mcp
- llms.txt: https://postfast.sh/llms.txt
- Privacy: https://postfast.sh/privacy, terms: https://postfast.sh/terms, data deletion: https://postfast.sh/data-deletion
- Other ways in: [Claude Code plugin](https://github.com/airdropfi/postfast-claude-plugin), [Gemini CLI extension](https://github.com/airdropfi/postfast-gemini-extension)

## Details

Prepare video posts for your own channels with [PostFast](https://postfast.sh) from Cursor or Grok Build. The assistant creates drafts; you review and press Post in PostFast.

## What it contains

| Component | Name | What it does |
|---|---|---|
| MCP server | `postfast` (`https://postfast.sh/api/mcp`) | The eight PostFast tools, authenticated with your API key as a Bearer header. |
| Rule | `rules/postfast.mdc` | Drafts only, user-supplied settings, honest channel list, keys never in chat. |
| Skill | `draft-a-video-post` | Find the channel, read its schema, upload media, create a draft, tell you to press Post in PostFast. |
| Skill | `check-post-status` | List posts for a date range and report states as returned. Read-only. |

## Sign-in: API key

This kit uses a PostFast API key sent as `Authorization: Bearer <key>`. PostFast's hosted MCP also supports OAuth 2.1 for clients that handle it.

1. Create a key in PostFast Settings, Public API section.
2. Put it in an environment variable that Cursor can see, for example `export POSTFAST_API_KEY=YOUR_API_KEY` in your shell profile, then restart Cursor. Never commit the key.

You can revoke the key in PostFast when you no longer need it.

## Install in Cursor

1. Add this to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

   ```json
   {
     "mcpServers": {
       "postfast": {
         "url": "https://postfast.sh/api/mcp",
         "headers": {
           "Authorization": "Bearer ${env:POSTFAST_API_KEY}"
         }
       }
     }
   }
   ```

2. Open Customize, then MCPs, and check that `postfast` shows its tools.
3. Optional, the rule: copy `rules/postfast.mdc` to `.cursor/rules/postfast.mdc` in your project. It is set to "Agent Decides", so Cursor applies it when you ask about PostFast.
4. Optional, the skills: copy the folders in `skills/` to `~/.cursor/skills/`.

Cursor asks you to approve MCP tool calls by default. Leave that on.

## Install in Grok Build

```bash
grok mcp add --transport http postfast https://postfast.sh/api/mcp --header "Authorization: Bearer YOUR_API_KEY"
```

Grok Build also loads `.cursor/mcp.json`.

## Use

Try the prompts in [examples/prompts.md](examples/prompts.md).

## What it will and will not do

- It creates drafts only and tells you to open PostFast and press Post now or Add to calendar. It never claims a post was published.
- YouTube and TikTok settings come from you, not from the assistant.
- It cannot delete posts.
- It reports post states exactly as PostFast returns them.
- It never asks you to paste a key into the chat.

## Data and privacy

The MCP server receives what the tools need: the captions, media URLs and settings you ask the agent to use, and the channel and post data on your PostFast account. This repository stores nothing and runs no code on your machine; it is a server address, Markdown instructions and a logo. See the [privacy policy](https://postfast.sh/privacy), [terms](https://postfast.sh/terms) and [data deletion](https://postfast.sh/data-deletion).

## Layout

```text
.cursor-plugin/plugin.json   Cursor Plugin manifest (also read by Grok Build)
mcp.json                     MCP server for Cursor (API key from POSTFAST_API_KEY)
.mcp.json                    the same server for Grok Build and other loaders
rules/postfast.mdc           rule
skills/                      two skills (SKILL.md)
assets/logo.png              1024x1024 logo
examples/prompts.md          example prompts
```

## Support

https://postfast.sh/mcp or support@postfast.sh.

## License

MIT for the files in this repository (see `LICENSE`). The PostFast service is governed by its terms.
