---
name: text-polish
description: 润色、翻译、总结、改写成多种风格。把草稿变成品。
---
# text-polish

对文本进行处理。根据 Arguments 中的操作类型执行：

## 操作类型

### polish（润色）
- 修正语法和错别字
- 优化表达流畅度
- 保持原意不变
- 标注重大改动

### translate（翻译）
- 中→英 或 英→中 自动检测
- 技术术语保留英文（如 API、SDK）
- 保持格式（markdown / 纯文本）

### summarize（总结）
- 输出 3 句话版本 + 1 句话版本
- 保留关键数字和人名

### rewrite（改写风格）
支持目标风格：
- `professional` — 正式商务
- `casual` — 轻松口语化
- `concise` — 极致简洁（微博/推特长度）
- `tech-doc` — 技术文档风格

## 规则
- 不添加原文没有的事实
- 改写时标注风格变化
- 代码块原样保留不翻译
