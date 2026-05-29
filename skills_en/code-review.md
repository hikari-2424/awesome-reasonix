---
name: code-review
description: Review code across 4 dimensions — logic, security, performance, style.
---
# code-review

Review target files or diffs, output structured feedback.

## Dimensions
- Logic: edge cases, null handling, error handling, dead code
- Security: SQL injection, XSS, hardcoded keys, path traversal
- Performance: N+1 queries, unnecessary loops, large object copies
- Style: naming, function length, nesting depth, comments

Output format: 🔴 Must fix · 🟡 Suggested · 🟢 Good
