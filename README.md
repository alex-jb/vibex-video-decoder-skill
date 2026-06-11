# vibex-video-decoder-skill

> **English** | [中文](README.zh-CN.md)


**Claude Skill: paste a Douyin (抖音) / 小红书 URL → Claude calls VibeXForge → AI watches the video → returns hook breakdown + 3 remix scripts.**

The first **Chinese-short-form viral-hook analyzer** as a [Claude Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview). Wraps [vibexforge.com/api/video-decode](https://vibexforge.com/tools/video-decode) so users in `claude.ai`, Claude Code, or any MCP-aware client can analyze short-form videos without leaving the conversation.

[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## Install

### claude.ai (Skills Marketplace)
Coming soon — pending Anthropic Skills Marketplace listing review. Watch this repo or `claude.ai/skills`.

### Claude Code (manual install)
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/alex-jb/vibex-video-decoder-skill ~/.claude/skills/vibex-video-decoder
```

Restart Claude Code. The skill auto-activates when you paste a Douyin / Xiaohongshu URL.

### MCP / other clients
The skill is a single `SKILL.md` with no external runtime dependencies. Any tool that understands the Anthropic Skill format (frontmatter + markdown body) can use it.

## What it does

```
You: 这条抖音为什么爆?https://v.douyin.com/iJSxxxxxx/

Claude (via this skill):
   ↓ POST vibexforge.com/api/video-decode { url: "..." }
   ↓ VibeXForge sidecar pulls mp4 from Douyin
   ↓ Gemini 2.5 Flash watches + listens (audio + visual)
   ↓ returns structured JSON

Claude (formatting):
   📊 Virality: 78/100 (Gemini honest estimate)
   🎣 Hook formula: "shock + question"
   📝 First-3s transcript: "..."
   🎬 Rhythm beats: [3.0, 7.2, 12.5, 18.1] avg 4.5s/shot
   📞 CTA: follow @ 47s — "..."
   💗 Emotional hook: 求职焦虑
   ✨ 3 remix scripts: 1. ... 2. ... 3. ...
```

## Cost

- VibeXForge backend pays Gemini 2.5 Flash ~$0.015 per 60s video
- Web UI free tier: 3 decodes / browser / day
- $5 unlocks 100 decodes via Stripe (never expires)
- This Skill calls the public endpoint anonymously — same free tier applies

## Why a Skill not a website

A Claude user analyzing a viral hook doesn't want to leave the conversation. They have context (a marketing plan they're drafting, a content calendar they're building). A skill keeps the analysis in-thread:
- Claude sees the URL → calls API → reads JSON → translates into a structured Chinese reply
- The 3 remix scripts land in the same conversation, ready for follow-up "make script 2 punchier" iteration
- No tab-switching, no copy-paste lag

## Privacy / legal

- Sidecar acts on individual user-triggered URLs only — no bulk scraping
- Gemini 2.5 Flash File API caches uploads 24h then deletes
- No data persisted on VibeXForge backend after the analysis JSON is returned
- VibeXForge is positioned for personal / research / indie scale, NOT B2B SaaS over Chinese platforms (Douyin ToS section 8.2 forbids automated access)

## When NOT to use

- YouTube, Vimeo, Twitch, long-form Western video — different audience analysis model
- Static images, audio-only, live streams
- B2B scraping at volume — get a paid API like TikAPI ($99/mo) instead

## Related

- [vibex](https://github.com/alex-jb/vibex) — VibeXForge Next.js app + API
- [vibex-video-extractor](https://github.com/alex-jb/vibex-video-extractor) — Python sidecar that pulls mp4 from URL
- [council-diff](https://github.com/alex-jb/council-diff) — 5-voice + Fable 5 Oracle decision framework
- [solo-founder-os](https://github.com/alex-jb/solo-founder-os) — 11-agent OSS stack

## License

MIT. SKILL.md is the source of truth; everything else is reference docs.

## Roadmap

- [x] v0.1.0 — Douyin / TikTok URL support, mp4 file upload, Gemini 2.5 Flash analysis
- [ ] v0.2.0 — Xiaohongshu URL support (Phase 2 in sidecar)
- [ ] v0.3.0 — MCP server wrapper for non-Claude clients
- [ ] v0.4.0 — Local hook-archetype taxonomy CSV (12 archetypes Alex curates from first 1000 decodes)
