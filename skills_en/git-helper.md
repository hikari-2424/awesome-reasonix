---
name: git-helper
description: Git helper — commit messages, PR descriptions, history analysis, conflict resolution.
---
# git-helper

Assist with daily Git operations.

## Operations

### commit
1. Run `git diff --staged` to view changes
2. Generate a Conventional Commits message:
   ```
   <type>(<scope>): <short description>
   ```
   type: feat / fix / refactor / docs / test / chore / style / perf
3. Show suggested message, commit after user confirmation

### pr
1. Run `git log main..HEAD --oneline`
2. Generate PR description template

### log
1. Run `git log --oneline -n <count>`
2. Group by type, flag suspicious commits

### conflict
1. List conflicted files
2. Analyze conflict causes
3. Suggest resolution strategy

## Rules
- Never auto-push — always require manual confirmation
- Don't modify .git/ directory
- Commit message first line ≤ 72 characters
- Don't amend pushed commits
