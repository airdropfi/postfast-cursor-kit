---
name: draft-a-video-post
description: Create a draft video post on the user's PostFast channels (YouTube, TikTok and others), after the user has given the required settings. Use when the user asks to draft, prepare, post or schedule a video or text post through PostFast. It only creates a draft; the user publishes in PostFast.
license: MIT
compatibility: Needs the PostFast MCP server connected and signed in. Creates drafts only; it never publishes or schedules.
---

# Draft a video post with PostFast

The rule of this skill: the AI creates a DRAFT. Nothing is published until the user opens the draft in PostFast and presses Post now or Add to calendar.

## Rules that always apply

1. **Drafts only.** `createDraftPostTool` creates a DRAFT on the user's channels. It never publishes or schedules. Never say a post was published or scheduled. After creating a draft, tell the user to open PostFast and press Post now or Add to calendar.
2. **No deletion.** You cannot delete posts. If asked, say so and point the user to PostFast.
3. **Channel first.** Call `integrationList` (or `groupList` for customer groups) to find the channel. Call `integrationSchema` for that channel type before building settings, and follow its rules.
4. **Settings come from the user.** For YouTube (title, privacy public/unlisted/private, made-for-kids) and TikTok (privacy, comments, duet, stitch, commercial-content disclosure) ask the user for each choice that the schema requires. Never guess them and never pick a more public option on their behalf.
5. **Media.** Use `uploadFromUrlTool` to copy a public image or video URL into the media library, and use the media it returns. Only use URLs the user gave you.
6. **Report status as returned.** `postsListTool` lists posts between two UTC dates with state, channel and settings. Say exactly what it returned and do not infer states it did not show. Use `postSettingsTool` only to change settings of an unpublished post or draft, as the user asks.
7. **Honest channels.** Live channels are YouTube, TikTok, Bluesky, WordPress, DEV and Hashnode. Do not offer X, LinkedIn, Instagram, Facebook, Threads, Reddit, Pinterest, Telegram or others.
8. **Keys stay out of chat.** Never ask the user to paste an API key or token into the conversation. An API key is created by the user in PostFast Settings, in the Public API section.

## Steps

1. Call `integrationList` (or `groupList` first when the user mentions a customer group). Pick the channel the user named. If it is unclear, ask. Only channels PostFast returns exist.
2. Call `integrationSchema` for that channel type and read its settings and rules (YouTube title, privacy, madeForKids; TikTok privacy, comment, duet, stitch, disclosure). If the schema needs a provider lookup such as tags or publications (WordPress, Hashnode), call `triggerTool`; it is read-only.
3. Ask the user for every required setting you do not already have from them. For YouTube and TikTok these choices must come from the user.
4. If the media is a public URL, call `uploadFromUrlTool` and use what it returns. If the user has no media URL, say so; do not invent one.
5. Show the user the caption or title, description, media and each setting exactly as they will be sent.
6. Call `createDraftPostTool` once with those values.
7. Tell the user a draft was created and that they must open PostFast, review it and press Post now or Add to calendar. Do not say it was published or scheduled. For TikTok, remind them the privacy and disclosure choices are shown in the PostFast composer.
