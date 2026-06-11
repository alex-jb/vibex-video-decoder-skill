# Security policy

## Reporting a vulnerability

Email **xji1 [at] mail.yu.edu** with subject line `vibex-video-decoder-skill security`. Include:

- The vulnerability description
- Reproduction steps
- The Claude version where it triggered

I'll acknowledge within 72 hours.

## Threat model

This repo contains a single Claude Skill (`SKILL.md`) plus reference docs and a hook-archetype taxonomy CSV. It is **not** an executable. It contains no API keys, no secrets, no credentials.

The skill itself instructs Claude to call a public HTTPS endpoint at `vibexforge.com/api/video-decode`. The actual computation runs on VibeXForge backend infrastructure (not this repo), routed through Gemini 2.5 Flash on Google's File API.

What this repo's `SKILL.md` protects against:
- ✅ Activation on unrelated long-form content (explicit allowlist of URL patterns)
- ✅ Activation on non-video media (excludes images, audio-only, livestreams)
- ✅ Prompt-injection of `SKILL.md` content by avoiding any executable code or shell commands in instructions

What it does **not** protect against:
- ❌ Misuse of the vibex backend (rate-limit, billing fraud, ToS violations on Douyin/小红书). The vibex backend has its own quota + Stripe paywall. Skill-level abuse must be handled there.
- ❌ A malicious operator who runs a fork of this skill against a hostile vibex clone. Verify the `SKILL.md` instruction points to `vibexforge.com` before installing skill forks.

## Recommendation for users

- Install only from official sources: `git clone https://github.com/alex-jb/vibex-video-decoder-skill` or the Anthropic Skills Marketplace listing.
- If you find a fork on the Marketplace pointing to a different endpoint, report it.
- The skill calls a public endpoint anonymously — no auth, no PII. The endpoint receives only the URL or mp4 you provide.

## Dependencies

This repo has no runtime dependencies. The skill is a single markdown file. The CSV is reference data.

The vibex backend that the skill calls runs on:
- Next.js 16 + React 19 + Tailwind 4
- Supabase (Postgres with RLS)
- Google Gemini 2.5 Flash File API
- Vercel-hosted edge functions

Security advisories on those upstream stacks are tracked separately at github.com/alex-jb/vibex.
