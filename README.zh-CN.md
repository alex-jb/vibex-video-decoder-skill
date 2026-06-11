# vibex-video-decoder-skill

> [English](README.md) | **中文**

**Claude Skill:粘抖音/小红书链接 → Claude 调 VibeXForge → AI 看视频 → hook 拆解 + 3 条仿写脚本。**

第一个**中文短视频病毒钩拆解 Claude Skill**。包装 [vibexforge.com/api/video-decode](https://vibexforge.com/tools/video-decode),让 `claude.ai`、Claude Code、或任何 MCP-aware 客户端的用户都能在对话里直接分析短视频,不用切窗口。

[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## 安装

### claude.ai (Skills Marketplace)
即将上线 — 等 Anthropic Skills Marketplace 审核。关注本 repo 或 `claude.ai/skills`。

### Claude Code (手动安装)
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/alex-jb/vibex-video-decoder-skill ~/.claude/skills/vibex-video-decoder
```

重启 Claude Code。粘抖音/小红书 URL 时 skill 自动激活。

### MCP / 其他客户端
Skill 是单个 `SKILL.md`,无外部 runtime 依赖。任何懂 Anthropic Skill 格式(frontmatter + markdown body)的工具都能用。

## 干什么

```
你: 这条抖音为什么爆?https://v.douyin.com/iJSxxxxxx/

Claude (通过 skill):
   ↓ POST vibexforge.com/api/video-decode { url: "..." }
   ↓ VibeXForge sidecar 从抖音拉 mp4
   ↓ Gemini 2.5 Flash 看 + 听 (音频 + 视觉)
   ↓ 返回结构化 JSON

Claude (格式化):
   📊 病毒指数: 78/100 (Gemini 自评,不灌水)
   🎣 Hook 公式: "shock + question"
   📝 前 3 秒 transcript: "..."
   🎬 节奏切点: [3.0, 7.2, 12.5, 18.1] 平均 4.5s/镜头
   📞 CTA: follow @ 47s — "..."
   💗 情感钩: 求职焦虑
   ✨ 3 条仿写脚本: 1. ... 2. ... 3. ...
```

## 成本

- VibeXForge 后端付 Gemini 2.5 Flash ~$0.015 / 60s 视频
- 网站免费额度:3 次/浏览器/天
- $5 解锁 100 次,永不过期
- 这个 Skill 用公共 endpoint anonymous 调用,同样的免费额度

## 为什么是 Skill 不是网站

Claude 用户分析 viral hook 时不想离开对话。他们有 context(正在草拟营销计划、内容日历)。Skill 把分析留在 thread:
- Claude 看到 URL → 调 API → 读 JSON → 翻译成结构化中文回复
- 3 条仿写脚本直接落在同一对话,可以追问"把第 2 条改尖锐点"
- 不用切 tab、不用复制粘贴

## 隐私 / 法律姿态

- Sidecar 只对用户单次主动触发的 URL 操作 — 不批量爬虫
- Gemini 2.5 Flash File API 缓存上传 24 小时后删除
- VibeXForge 后端不持久化分析结果以外的任何东西
- VibeXForge 定位 personal / 研究 / indie scale,**不**适合 B2B SaaS 大规模抓中国平台(抖音 ToS 第 8.2 条禁止自动化访问)

## 什么时候**不**用

- YouTube、Vimeo、Twitch、长视频西方内容 — 受众分析模型不同
- 静态图片、纯音频、直播
- B2B 大规模抓取 — 用付费 API 如 TikAPI($99/月)

## 相关

- [vibex](https://github.com/alex-jb/vibex) — VibeXForge Next.js 应用 + API
- [vibex-video-extractor](https://github.com/alex-jb/vibex-video-extractor) — 从 URL 拉 mp4 的 Python sidecar
- [council-diff](https://github.com/alex-jb/council-diff) — 5-voice + Fable 5 Oracle 决策框架
- [solo-founder-os](https://github.com/alex-jb/solo-founder-os) — 11-agent OSS 栈

## License

MIT。SKILL.md 是 source of truth,其他都是参考文档。

## Roadmap

- [x] v0.1.0 — 抖音 / TikTok URL 支持、mp4 文件上传、Gemini 2.5 Flash 分析
- [ ] v0.2.0 — 小红书 URL 支持(sidecar Phase 2)
- [ ] v0.3.0 — MCP server wrapper for non-Claude 客户端
- [ ] v0.4.0 — 本地 hook 分类 CSV(Alex 从前 1000 次拆解里精选 12 个 archetype)
