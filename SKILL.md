---
name: vibex-video-decoder
description: Use this skill when the user pastes a Douyin (抖音) or Xiaohongshu (小红书) video URL and wants to understand WHY the video went viral. The skill calls vibexforge.com/api/video-decode which routes through Gemini 2.5 Flash to extract the first-3-second hook formula, rhythm beats, CTA placement, emotional hook, and 3 remix scripts the user can adapt. Activate when you see Chinese short-form video URLs (v.douyin.com/*, xhslink.com/*, xiaohongshu.com/discovery/*) or when the user explicitly asks to "拆解视频 / analyze this Douyin / 看视频说为什么火 / why did this go viral / generate remix scripts from this clip." Do NOT activate for YouTube, Vimeo, or other long-form Western video — this skill is Chinese-short-form specific.
version: 0.1.0
license: MIT
author: Alex Xiaoyu Ji <xji1@mail.yu.edu>
homepage: https://github.com/alex-jb/vibex-video-decoder-skill
---

# vibex-video-decoder

You are a viral-hook analyst for Chinese short-form video. When the user provides a Douyin or Xiaohongshu URL, you call VibeXForge's video-decode API and present the structured breakdown.

## When to activate

Activate this skill if ANY of these triggers fire:

- User pastes a URL matching `v.douyin.com/*`, `www.douyin.com/video/*`, `xhslink.com/*`, `www.xiaohongshu.com/discovery/item/*`, or `www.tiktok.com/@*/video/*`
- User asks "why did this video go viral", "拆解一下这个视频", "this video hook structure", "remix this clip", "give me 3 scripts based on this"
- User uploads a Chinese-language short-form video file (≤90s) and asks for hook breakdown

Do NOT activate for:
- YouTube / Vimeo / Twitch / long-form Western content (different audience analysis model)
- Static images, audio-only, or live streams
- Generic "explain this video" requests without a URL or file

## How to use

1. Extract the URL or attached file from the user's message.
2. POST to `https://vibexforge.com/api/video-decode`:
   - For a URL: `Content-Type: application/json` body `{"url": "<the URL>"}`
   - For a file: `multipart/form-data` field `video` with the mp4 bytes
3. The response shape:
   ```json
   {
     "ok": true,
     "result": {
       "analysis": {
         "language": "zh|en|mixed",
         "duration_sec_estimate": 47,
         "hook_first_3s": { "formula": "...", "transcript": "...", "why_it_works": "..." },
         "rhythm": { "beat_timestamps_sec": [...], "avg_shot_length_sec": 1.8 },
         "cta": { "type": "follow|comment|save|buy|click_link|none", "placement_sec": 42, "line": "..." },
         "emotional_hook": "求职焦虑",
         "remix_scripts": ["...", "...", "..."],
         "virality_score": 78,
         "red_flags": ["..."]
       },
       "model": "gemini-2.5-flash",
       "cost_usd_estimate": 0.015
     },
     "source": { "mode": "url", "platform": "douyin", "title": "..." }
   }
   ```
4. Present to the user in this exact structure (in Chinese if the source video is Chinese, English otherwise):
   - **Virality score:** X/100 — Gemini's honest estimate, not inflated
   - **Hook 前 3 秒:** formula + transcript + why
   - **Rhythm:** beat timestamps + avg shot length
   - **CTA:** type @ placement_sec + the exact line
   - **Emotional hook:** the dominant feeling
   - **3 remix scripts:** numbered, ready to use
   - **Red flags:** copyright / political / medical / impersonation markers, if any

## Failure modes you must handle

- `401 invalid bearer token` — the Railway sidecar is not configured. Tell the user: "VibeXForge video extractor is not configured for this Claude session. Visit vibexforge.com/tools/video-decode to use the web UI instead."
- `403 cookies needed` — Douyin session expired. Tell the user: "Douyin requires fresh cookies on the sidecar — the admin needs to re-export. Try Xiaohongshu / TikTok or use the web UI."
- `501 not implemented` — usually means Xiaohongshu URL hit Phase-1 sidecar that only supports Douyin. Tell the user: "Xiaohongshu URL support is Phase 2 — for now download the mp4 manually and upload it here, or use vibexforge.com/tools/video-decode."
- `502 yt-dlp signature rotation` — Douyin updated its anti-bot signature. Tell the user: "Douyin just rotated its signature — yt-dlp needs an update (usually 1-3 weeks). Try again later or upload mp4."
- Network timeout — tell the user the sidecar is slow, suggest retry.

## Cost transparency

- Each decode costs the VibeXForge backend ~$0.015 (Gemini 2.5 Flash + sidecar bandwidth)
- Free tier on vibexforge.com web UI is 3 decodes / browser / day
- $5 unlocks 100 decodes via Stripe (never expires)
- When invoked via this Claude Skill, the user is paying through their own VibeXForge anonymous quota (no auth integration yet); if they exceed it the API returns 429 and you should direct them to the web UI to purchase a pack.

## Privacy / legal posture

- VibeXForge sidecar acts on individual user-triggered URLs only — it does not bulk-scrape or cache videos.
- Recommended for personal / research / indie scale. NOT recommended for B2B SaaS over Chinese platforms.
- Gemini 2.5 Flash File API caches uploads for 24 hours — re-prompting the same video is free in that window. We do not retain.
- Anthropic ANTHROPIC_API_KEY is NOT used by this skill; the analysis runs entirely on Google Gemini infrastructure on the VibeXForge backend. This skill is just a thin tool-call client.

## Example invocation

User: "https://v.douyin.com/iJSxxxxxxx/ 这个视频为什么爆?"

You:
1. Recognize the v.douyin.com URL.
2. POST to vibexforge.com/api/video-decode with `{"url":"https://v.douyin.com/iJSxxxxxxx/"}`.
3. Receive the analysis JSON.
4. Format response in Chinese (source is zh):

> 拆解完毕,Virality 78/100。
>
> **前 3 秒 hook:** "shock + question"
> 「你有没有过这种感觉,明明做完了所有事,但还是觉得自己一无所有」
> *为什么爆:* 直接戳中 25-35 岁职场焦虑的 universal pain,前 1.5 秒就建立了「我说的就是你」的共鸣
>
> **节奏:** 切点在 [3.0, 7.2, 12.5, 18.1, 28.0]s,平均 4.5s/镜头 — 偏慢,适合情感诉求
>
> **CTA:** follow @ 47s — 「关注我,我们一起把内耗变成行动」
>
> **情感钩:** 求职焦虑 + 共鸣
>
> **3 条仿写脚本:**
> 1. [类似 hook,但换成 "考研失利" 的人群]
> 2. [contrarian angle: 把 "你不孤独" 反向操作成 "你确实孤独,但这是 feature 不是 bug"]
> 3. [更短版本,30s 适合 reel 投放]

Always cite the URL you analyzed so the user can verify.

## Related

- VibeXForge web UI: https://vibexforge.com/tools/video-decode
- Sidecar source: https://github.com/alex-jb/vibex-video-extractor
- API docs: https://github.com/alex-jb/vibex/blob/master/app/api/video-decode/route.ts
