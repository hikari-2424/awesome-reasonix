# Contributing

PRs and Issues welcome.

## What gets in

- **Skills** — markdown files that work with Reasonix. Must have frontmatter (`name`, `description`).
- **MCP servers** — known to work with Reasonix. Include setup instructions.
- **Guides** — tutorials, workflows, tips. Not "how to install Reasonix" basics.

## What doesn't

- Link collections with no original content.
- Broken, untested, or abandoned projects.
- AI-generated content that hasn't been verified by a human.
- Paid products or services.
- Duplicates — search first.

## How to submit

### Via Issue (quickest)

Use the [submission template](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-submit.yml). Paste your Skill's markdown. Automated checks run immediately.

### Via PR

1. Fork the repo.
2. Add your Skill file to `skills/`.
3. Add an entry to `registry.json`.
4. PR title: `Add: <name>`.

One entry per PR.

## Checks

Submissions go through automated validation:

- Frontmatter present (`name`, `description`)
- Content ≥ 100 characters
- No dangerous shell patterns (`rm -rf /`, `curl | sh`, etc.)

Pass → published immediately. Fail → comment with the reason.

## Format

Skill files:

```markdown
---
name: my-skill
description: What it does, one line.
---

# my-skill

Instructions for the agent. Clear, specific, tested.
```

Registry entries:

```json
{
  "id": "my-skill",
  "type": "skill",
  "name": "my-skill",
  "category": "开发工具",
  "description": "What it does.",
  "url": "https://github.com/hikari-2424/awesome-reasonix/blob/main/skills/my-skill.md",
  "rawUrl": "https://raw.githubusercontent.com/hikari-2424/awesome-reasonix/main/skills/my-skill.md",
  "tags": ["tag1", "tag2"],
  "installCmd": "curl -o .reasonix/skills/my-skill.md https://raw.githubusercontent.com/hikari-2424/awesome-reasonix/main/skills/my-skill.md"
}
```

## License

By submitting, you agree to publish under [CC0 1.0](LICENSE).
