---
name: file-organizer
description: Classify, deduplicate, and organize files in a directory.
---
# file-organizer

Organize files in a directory.

## Operations

### classify
Sort files by extension into subdirectories:
```
images/   → .jpg .png .gif .svg .webp
docs/     → .pdf .doc .docx .xls .xlsx .ppt .pptx .txt .md
archives/ → .zip .rar .7z .tar .gz
code/     → .py .js .ts .go .rs .java .html .css .json .yaml
media/    → .mp4 .mp3 .avi .mov .wav
others    → others/
```

### deduplicate
- Compare filename + size, flag suspected duplicates
- Does not auto-delete — only lists suggestions

### tree
- Generate readable directory structure with file count and total size

### clean-empty
- Recursively find empty directories, list then ask before deleting

## Rules
- All destructive operations preview first, execute after confirmation
- Skip hidden files and system directories
