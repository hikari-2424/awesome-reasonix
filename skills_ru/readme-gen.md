---
name: readme-gen
description: 分析项目代码自动生成专业 README.md，含安装、使用、API、贡献指南。
---
# readme-gen

分析项目结构和代码，生成专业 README.md。

## 操作

### generate（生成 README）
1. 扫描项目结构：
   - `package.json` / `Cargo.toml` / `go.mod` / `requirements.txt` → 技术栈和依赖
   - `src/` / `lib/` / `app/` → 项目结构
   - `.gitignore` → 构建产物
   - 入口文件 → 启动方式
   - 测试目录 → 测试框架
2. 分析代码：
   - 主要 API / 导出函数
   - 配置选项
   - 环境变量
3. 生成 README：
   ```markdown
   # [项目名]

   [一句话描述]

   ## 安装
   ## 快速开始
   ## API / 使用
   ## 配置
   ## 开发
   ## 测试
   ## 贡献
   ## 许可
   ```
4. 展示预览，确认后写入 README.md

### update（更新 README）
1. 读取现有 README
2. 对比项目变化（新依赖、新文件、新功能）
3. 只更新变化的部分，保留手写内容
4. 标注哪些部分自动生成、哪些手动维护

## 规则
- 不覆盖用户手写的项目描述和徽章
- 安装命令必须可运行（验证依赖文件存在）
- API 部分从代码注释/JSDoc 提取，不编造
- 如果 README 已存在且内容丰富，用 update 而非 generate
