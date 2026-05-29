---
name: file-organizer
description: 按规则整理文件：分类、去重、清理、生成目录结构。
---
# file-organizer

整理指定目录下的文件。

## 支持的操作

### classify（分类）
按扩展名归类到子目录：
```
images/   → .jpg .png .gif .svg .webp
docs/     → .pdf .doc .docx .xls .xlsx .ppt .pptx .txt .md
archives/ → .zip .rar .7z .tar .gz
code/     → .py .js .ts .go .rs .java .html .css .json .yaml
media/    → .mp4 .mp3 .avi .mov .wav
其他      → others/
```

### deduplicate（去重）
- 对比文件名+大小，标记疑似重复
- 不自动删除，只列出建议

### tree（目录树）
- 生成可读的目录结构，标注文件数量和总大小

### clean-empty（清理空目录）
- 递归查找空目录，列出后询问是否删除

## 规则
- 所有破坏性操作先展示计划，经确认后执行
- 不操作隐藏文件和系统目录
