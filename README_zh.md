# Awesome Reasonix

🌐 **[浏览技能 →](https://hikari-2424.github.io/awesome-reasonix-site/)**

[Reasonix](https://github.com/esengine/reasonix) 的开源社区技能中心。非 Reasonix 官方项目——独立社区。

[English](README.md) · 中文 · [Русский](README_ru.md)

[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Discussions](https://img.shields.io/badge/discussions-open-6366f1)](https://github.com/hikari-2424/awesome-reasonix/discussions)

---

## 这是什么

Reasonix 支持通过 **Skill** 扩展能力——把 markdown 文件放入 `.reasonix/skills/` 即可。本仓库是社区收集、评分、分享这些 Skill 的地方。

**痛点**：每个人都自己写 git 助手、代码审查、文件整理。**解决**：写一次，分享到这里，让其他人评分和改进。

---

## 安装一个 Skill

```bash
curl -o .reasonix/skills/file-organizer.md https://raw.githubusercontent.com/hikari-2424/awesome-reasonix/main/skills/file-organizer.md
```

运行 Reasonix，输入 `/skill file-organizer`。完成。

两个示例：

| Skill | 做什么 |
|---|---|
| [file-organizer](skills/file-organizer.md) | 文件分类、查重、目录树、清理空目录。 |
| [git-helper](skills/git-helper.md) | Conventional Commits、PR 描述、历史分析。 |

→ **[在站点查看全部 15+ 技能](https://hikari-2424.github.io/awesome-reasonix-site/)**

---

## MCP 服务器

以下 [MCP](https://modelcontextprotocol.io/) 服务器与 Reasonix 兼容：

- [filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) — 文件系统访问，可配路径范围。
- [github](https://github.com/modelcontextprotocol/servers/tree/main/src/github) — 仓库、PR、Issue、代码搜索。
- [puppeteer](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer) — 无头浏览器自动化。
- [memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) — 知识图谱，跨会话持久化。

---

## 参与共建

用[提交模板](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-submit.yml)开 Issue 或直接提 PR。

自动化检查校验格式和安全。通过 → 即时发布。不通过 → 评论说明原因。无需人工审核。

差评过多或被多次举报的 Skill 自动隐藏。

完整指南：[如何创建 Skill](guides/how-to-create-a-skill.md) · [CONTRIBUTING.md](CONTRIBUTING.md)

---

## 评价与举报

评价或举报——开 GitHub Issue，无需额外账号。

- [提交评价](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-review.yml)
- [举报问题](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-flag.yml)

---

## 安全提醒

Skill 在你的机器上运行。安装前：读文件、看评分、先测试。

[SECURITY.md](SECURITY.md) · [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

---

## 链接

- [社区站点](https://hikari-2424.github.io/awesome-reasonix-site/)
- [GitHub Issues](https://github.com/hikari-2424/awesome-reasonix/issues)
- [GitHub Discussions](https://github.com/hikari-2424/awesome-reasonix/discussions)
- [Reasonix](https://github.com/esengine/reasonix)
- [Reasonix Discord](https://discord.gg/XF78rEME2D)

---

## 许可

[MIT](LICENSE)。本仓库的 Skill 文件可自由使用、修改和分享。
