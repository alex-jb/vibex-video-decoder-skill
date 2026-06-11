# Reddit r/ClaudeAI — vibex-video-decoder-skill

**Subreddit:** r/ClaudeAI · ~80k members as of 2026-06
**Best post window:** weekday 9am-12pm ET
**Flair:** Show / Other

---

## Title

I built a Claude Skill that lets claude.ai watch Douyin/小红书 videos in-thread (12-archetype hook taxonomy, MIT)

## Body

I needed a way to analyze viral Chinese short-form video without leaving claude.ai. Existing tools (飞瓜, 蝉妈妈) are B2B-priced ¥3,000-30,000/year and don't return structured data. English tools (Hookline, TikBuddy) don't speak Chinese.

So I wrapped my own pipeline as a Claude Skill.

How it works:

1. You paste a Douyin URL into claude.ai
2. Skill activates (frontmatter description triggers on `v.douyin.com/*`, `xhslink.com/*`, etc.)
3. Claude calls `vibexforge.com/api/video-decode` — a Python sidecar on Railway pulls the mp4, Gemini 2.5 Flash watches + listens, returns structured JSON
4. Claude formats the response inline:

```
📊 Virality: 78/100 (Gemini honest estimate)
🎣 Hook formula: "shock + question"
🎯 Archetype: Pain Reveal · 痛点直击
📝 First-3s: "你有没有过这种感觉..."
🎬 Beats: [3.0, 7.2, 12.5, 18.1] avg 4.5s/shot
📞 CTA: follow @ 47s
💗 Emotional: 求职焦虑
✨ 3 remix scripts (numbered)
```

The 12-archetype hook taxonomy (shock_question, pain_reveal, speed_run, before_after, contrarian_take, list_with_count, story_arc, authority_proof, viewer_jealousy, ai_dare, emotional_resonance, controversy_bait) is in the repo as CSV. Gemini classifies against the enum directly during decode so the badge is deterministic, not vibes.

Why a Skill specifically (not a website):

Claude users analyzing viral hooks are usually drafting marketing plans or content calendars. Leaving the thread breaks flow. The 3 remix scripts land in-conversation, ready for "make script 2 punchier" iteration. No tab-switch lag.

What I learned designing the SKILL.md frontmatter:

- The `description` field is what makes Claude decide to use the skill. URL pattern allowlist works well as a trigger.
- Explicit "do NOT activate for YouTube/Vimeo/Twitch" is important — otherwise the model gets confused when someone pastes any video URL.
- Including the failure-mode fallback ("if API returns 501, suggest web UI workaround") prevents Claude from going off-script.

Honest about coverage:

✅ Douyin / TikTok — works (the sidecar runs yt-dlp + ~10min/month cookie rotation)
❌ Xiaohongshu URL — Phase 2 (yt-dlp has no XHS extractor). Workaround: download .mp4 manually and use the web UI.

Free tier: 3 decodes / browser / day. $5 unlocks 100 (never expire). Backend pays ~$0.015/decode for Gemini 2.5 Flash.

MIT, sources:
- The Skill: github.com/alex-jb/vibex-video-decoder-skill
- The sidecar: github.com/alex-jb/vibex-video-extractor (Python + FastAPI + yt-dlp on Railway)
- The web UI: github.com/alex-jb/vibex (Next.js + Gemini integration)
- The hook taxonomy: vibexforge.com/hooks (12 archetype detail pages)

What activation patterns have worked for others building Skills? Curious specifically about edge cases — what makes Claude *not* use a skill when you wanted it to.
