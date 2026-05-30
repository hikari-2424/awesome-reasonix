---
name: project-scaffold
description: 快速搭建项目脚手架：选择技术栈 → 生成目录结构 → 配置工具链 → 初始化 Git。
---
# project-scaffold

交互式创建新项目，生成标准化的项目结构和配置。

## 操作

### scaffold（创建项目）
1. **询问技术栈**：
   - 语言：Python / TypeScript / Go / Rust
   - 类型：CLI / Web 后端 / Web 前端 / 库
   - 包管理：npm / yarn / pnpm / pip / poetry / cargo / go mod
2. **生成目录结构**：
   ```
   my-project/
   ├── src/              # 源码
   │   ├── index.ts      # 入口
   │   └── __tests__/    # 测试
   ├── .gitignore
   ├── package.json      # （或其他）
   ├── tsconfig.json     # （TypeScript）
   ├── .eslintrc.js      # （可选）
   ├── .prettierrc       # （可选）
   └── README.md
   ```
3. **初始化工具链**：
   - `git init` + 初始 commit
   - 安装开发依赖（eslint、prettier、测试框架）
   - 配置 CI 模板（GitHub Actions）
4. 输出项目概览和下一步

### add-ci（添加 CI 配置）
1. 检测项目类型
2. 生成 `.github/workflows/ci.yml`
3. 包含：lint → test → build 流水线

## 规则
- 不覆盖已存在的文件（除非用户确认）
- 版本号使用当前最新稳定版（先搜索确认）
- 生成后不自动执行安装——提示用户运行
- 所有模板文件保持最小化，不加不必要的注释
