---
name: changelog-generator
description: Generate CHANGELOG.md from git history. Groups by type, infers semver.
---
# changelog-generator

Generate CHANGELOG from git history.

## Operations

### generate
1. Run `git log <last-tag>..HEAD --oneline`
2. Group by Conventional Commits type (🚀 Features, 🐛 Bug Fixes, etc.)
3. Output in Keep a Changelog format

### release
1. Run generate
2. Suggest semver increment based on feat/fix ratio
