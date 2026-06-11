# Contributing

This is a Claude Skill — a single SKILL.md + supporting docs. Contributions welcome but kept lean.

## What we want

- **Better activation triggers.** The frontmatter `description` field drives when Claude decides to use this skill. If you find URL patterns or phrasing that should activate it but doesn't, open a PR to extend the description.
- **Hook archetype additions.** `hook-archetypes.csv` is the seed taxonomy. If you decode 50+ Chinese short-form videos and find a hook pattern not on the list, propose a new row. Real data > speculation.
- **More example invocations** in SKILL.md. The "Example invocation" section currently shows one Chinese vlog hook. Adding examples for Xiaohongshu food, fitness, etc. would make activation more reliable.
- **Translations.** SKILL.md is currently English-only (the structured prompt sits in English so Claude's tool-use reliability stays high). Multi-language README docs are welcome.
- **MCP server wrapper.** Roadmap item v0.3. Wrapping the Claude Skill format as an MCP server unlocks Cursor / Claude Code / any MCP-aware client.

## What we don't want

- **API-key collection.** This skill calls a public endpoint anonymously. It does not (and should not) prompt users for their own keys.
- **Heavy dependencies.** The skill should remain a single SKILL.md with optional supporting markdown. No JavaScript, no Python, no compiled binaries.
- **Activation on long-form Western video.** SKILL.md explicitly excludes YouTube / Vimeo / Twitch. The Chinese-short-form focus is the differentiator.
- **Bypassing the free-tier quota.** The free tier is 3 decodes/browser/day. Skills should not chain calls to circumvent it.

## Testing your changes

If you edit SKILL.md activation triggers:

1. Install locally: `cp SKILL.md ~/.claude/skills/vibex-video-decoder/SKILL.md`
2. Restart Claude Code
3. Paste a Douyin URL into Claude and see if the skill activates
4. Try edge cases: a similar-looking Chinese URL that isn't actually Douyin (e.g. weibo.com short link), make sure the skill correctly *doesn't* activate

If you edit `hook-archetypes.csv`:

1. Keep columns in this order: slug, display_en, display_zh, description_en, description_zh, common_in
2. slug must be lowercase_snake_case, no special chars
3. Match against vibex `lib/hook-archetypes.ts` — the TS export must stay in sync with the CSV
4. Open a paired PR to vibex when you add an archetype

## Reporting bugs

GitHub Issues. Include:
- The URL you tested (or a similar non-personal one that reproduces)
- The Claude version (e.g. claude.ai, Claude Code, Claude Desktop, version number)
- What you expected
- What actually happened

## Reporting security issues

See [SECURITY.md](SECURITY.md).
