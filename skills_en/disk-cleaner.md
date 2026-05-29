---
name: disk-cleaner
description: Scan and clean disk — temp files, cache, duplicates, large files.
---
# disk-cleaner

Scan directories and find redundant files to clean.

## Operations

### scan
1. Scan target directory
2. Classify: temp files (.tmp, .cache, .log), dependency caches (node_modules, __pycache__), backups (.bak, .old), duplicates, large files (>100MB)
3. Output categorized statistics

### clean
1. Show cleanup plan based on scan results
2. User confirms, then delete category by category
3. Safest files first (temp), then requires confirmation (duplicates)

### duplicate
1. Compare filename + size
2. List suspected duplicates with suggestions
3. Never auto-delete

## Rules
- All destructive operations preview first, confirm before executing
- Never touch system directories
- Never touch .git/
- Large files: report only, never auto-delete
- Skip locked/in-use files
