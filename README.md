# Awesome Reasonix

🌐 **[Browse Skills →](https://hikari-2424.github.io/awesome-reasonix-site/)**

Open-source community Skill hub for [Reasonix](https://github.com/esengine/reasonix). Not affiliated with Reasonix — an independent community project.

[English](README.md) · [中文](README_zh.md) · [Русский](README_ru.md)

[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Discussions](https://img.shields.io/badge/discussions-open-6366f1)](https://github.com/hikari-2424/awesome-reasonix/discussions)

---

## What is this

Reasonix lets you extend it with **Skills** — markdown files dropped into `.reasonix/skills/`. This repository is where the community collects, rates, and shares them.

**The problem**: everyone writes their own git helper, their own code reviewer, their own file organizer. **The solution**: write it once, share it here, let others rate and improve it.

---

## Install a Skill

```bash
curl -o .reasonix/skills/file-organizer.md https://raw.githubusercontent.com/hikari-2424/awesome-reasonix/main/skills/file-organizer.md
```

Then run Reasonix and type `/skill file-organizer`. That's it.

Two examples from the catalog:

| Skill | Does |
|---|---|
| [file-organizer](skills/file-organizer.md) | Classify files, find duplicates, tree view, clean empty dirs. |
| [git-helper](skills/git-helper.md) | Conventional Commits, PR descriptions, history analysis. |

→ **[See all Skills on the site](https://hikari-2424.github.io/awesome-reasonix-site/)**

---

## MCP Servers

These [MCP](https://modelcontextprotocol.io/) servers work with Reasonix:

- [filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) — filesystem access with path scoping.
- [github](https://github.com/modelcontextprotocol/servers/tree/main/src/github) — repos, PRs, issues, code search.
- [puppeteer](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer) — headless browser automation.
- [memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) — knowledge graph, persists across sessions.

---

## Contribute

Open a [submission issue](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-submit.yml) or send a PR with your Skill.

Automated checks validate format and safety. Pass → published immediately. Fail → comment explains why. No manual review.

Skills with low ratings or multiple flags are hidden automatically.

Full guide: [How to Create a Skill](guides/how-to-create-a-skill.md) · [CONTRIBUTING.md](CONTRIBUTING.md)

---

## Rate & Report

Rate a skill or report a problem — open a GitHub Issue. No extra accounts needed.

- [Submit a review](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-review.yml)
- [Report a problem](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-flag.yml)

---

## Safety

Skills run on your machine. Before installing: read the file, check the ratings, test first.

[SECURITY.md](SECURITY.md) · [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

---

## Links

- [Community Site](https://hikari-2424.github.io/awesome-reasonix-site/)
- [GitHub Issues](https://github.com/hikari-2424/awesome-reasonix/issues)
- [GitHub Discussions](https://github.com/hikari-2424/awesome-reasonix/discussions)
- [Reasonix](https://github.com/esengine/reasonix)
- [Reasonix Discord](https://discord.gg/XF78rEME2D)

---

## License

[MIT](LICENSE). Skill files in this repository are free to use, modify, and share.
