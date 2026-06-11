# HN Show launch — vibex-video-decoder-skill

**When to post:** Tue/Wed/Thu 8-10am ET
**Title:** Show HN: Claude Skill that lets claude.ai watch Douyin/小红书 videos in-thread

---

## Post body

Wrapped vibexforge.com/tools/video-decode as a Claude Skill so users in claude.ai, Claude Code, or any MCP-aware client can paste a Douyin / Xiaohongshu URL and get a hook breakdown + 3 remix scripts without leaving the conversation.

What it actually does:

```
You: 这条抖音为什么爆? https://v.douyin.com/iJSxxxxxx/

Claude (via skill):
   ↓ POST vibexforge.com/api/video-decode { url: "..." }
   ↓ Python sidecar pulls mp4 from Douyin (yt-dlp)
   ↓ Gemini 2.5 Flash watches + listens (audio + visual)
   ↓ returns structured JSON

Claude (formats):
   📊 Virality: 78/100
   🎣 Hook formula: "shock + question"
   🎯 Archetype: Pain Reveal · 痛点直击
   📝 First-3s: "你有没有过这种感觉..."
   🎬 Beats: [3.0, 7.2, 12.5, 18.1] avg 4.5s/shot
   📞 CTA: follow @ 47s — "..."
   💗 Emotional hook: 求职焦虑
   ✨ 3 remix scripts (numbered, ready to use)
```

What's interesting (maybe):

- **First** Chinese-short-form viral-hook analyzer as a Claude Skill. Per a GitHub landscape audit yesterday: 0 direct competitors. (Lots of English-only TikTok analyzers, lots of Chinese B2B dashboards. Nothing that's both Chinese + indie-priced + skill-format.)
- **12-archetype hook taxonomy** seeded from research on Chinese short-form virality patterns (shock_question / pain_reveal / speed_run / before_after / contrarian_take / list_with_count / story_arc / authority_proof / viewer_jealousy / ai_dare / emotional_resonance / controversy_bait). CSV in the repo. Gemini classifies against the enum directly so the badge is deterministic.
- **In-thread > tab-switch**: a Claude user drafting a marketing plan doesn't want to leave the conversation. The skill keeps the hook breakdown in-context for follow-up iteration ("make script 2 punchier", "what's the equivalent hook for B2B SaaS?").

Backend infra (separate repos, also OSS):

- vibex-video-extractor — Python sidecar on Railway ($5/mo), pulls mp4 from URL via yt-dlp
- vibex — Next.js + Gemini 2.5 Flash integration + Stripe paywall

Cost / quota:

- Free tier: 3 decodes / browser / day
- $5 unlocks 100 decodes (never expire)
- VibeXForge backend pays ~$0.015/decode (Gemini File API)

Honest coverage:

- ✅ Douyin / TikTok — works
- ❌ Xiaohongshu URL — Phase 2 (yt-dlp has no XHS extractor). Workaround: download .mp4 manually, upload to vibex web UI.

MIT, source: github.com/alex-jb/vibex-video-decoder-skill

Anyone using Skills in production for content analysis or creator workflows? Curious what activation patterns have worked.
