# X thread — vibex-video-decoder-skill

**Best post window:** weekdays 9-11am ET or 5-7pm ET

---

**1/**
Shipped the first **Chinese-short-form viral-hook analyzer as a Claude Skill**.

Paste a Douyin / 小红书 URL in claude.ai → Claude calls VibeXForge → AI watches video → returns hook breakdown + 3 remix scripts.

In-thread. No tab switching.

🔗 github.com/alex-jb/vibex-video-decoder-skill

**2/**
The case for Skill > website:

A Claude user drafting a marketing plan doesn't want to leave the conversation to analyze a viral clip. They have CONTEXT — a content calendar, a brief, follow-up questions.

Skill keeps the analysis in-thread. Replies stay coherent. Iteration is "make script 2 punchier" not "go fetch the URL again".

**3/**
What you get back:

```
📊 Virality: 78/100
🎣 Hook: "shock + question"
🎯 Archetype: Pain Reveal · 痛点直击
📝 First-3s transcript: "..."
🎬 Rhythm: [3.0, 7.2, 12.5] avg 4.5s/shot
📞 CTA: follow @ 47s
💗 Emotional: 求职焦虑
✨ 3 remix scripts (numbered)
```

**4/**
Backed by a 12-archetype hook taxonomy (CSV in the repo): shock_question, pain_reveal, speed_run, before_after, contrarian_take, list_with_count, story_arc, authority_proof, viewer_jealousy, ai_dare, emotional_resonance, controversy_bait.

Gemini classifies against the enum so the badge is deterministic, not vibes.

**5/**
Cost discipline:

- Free: 3 decodes / browser / day
- $5 = 100 decodes (never expire)
- Backend pays ~$0.015/decode (Gemini 2.5 Flash)

Indie price tier vs Chinese B2B competitors at ¥3,000-30,000/yr.

**6/**
Honest about coverage:

✅ Douyin / TikTok — works
❌ Xiaohongshu URL — Phase 2 (yt-dlp has no XHS extractor)

Workaround: download .mp4 manually, upload to vibexforge.com/tools/video-decode upload mode.

**7/**
Why this matters past the demo:

Every decode that lands an archetype is a data point. The CSV mutates from theory to data-validated standard over the first 1000 decodes.

v0.3 plan: publish the archetype-vs-virality-score Brier dataset (which archetypes actually correlate with high score).

**8/**
Source + install: github.com/alex-jb/vibex-video-decoder-skill

Anthropic Skills Marketplace listing pending review.

If you're building creator-workflow tools in Claude — install + tell me what activation triggers I missed.
