---
name: changelog-generator
description: 从 Git 提交历史自动生成结构化的 CHANGELOG.md。
---
# changelog-generator

从 Git 提交历史生成标准化的 CHANGELOG。

## 操作

### generate（生成 changelog）
1. 运行 `git log <上次版本tag>..HEAD --oneline` 获取提交列表
2. 按 Conventional Commits 类型分组：
   - ### 🚀 Features (feat)
   - ### 🐛 Bug Fixes (fix)
   - ### 🔧 Refactors (refactor)
   - ### 📝 Documentation (docs)
   - ### 🧪 Tests (test)
   - ### ⚡ Performance (perf)
   - ### 🧹 Chores (chore)
3. 每条格式：`- [描述](PR链接，如有)`
4. 生成完整 CHANGELOG：
   ```markdown
   # Changelog

   ## [版本号] - YYYY-MM-DD

   ### 🚀 Features
   - ...

   ### 🐛 Bug Fixes
   - ...
   ```
5. 可选：追加到现有 CHANGELOG.md 或覆盖

### release（准备发版）
1. 运行 generate
2. 自动推断版本号（根据 feat/fix 比例建议 semver 增量）
3. 建议下一步：`git tag vX.Y.Z`、`git push --tags`

## 规则
- 不自动修改文件，先展示内容经确认后写入
- 如果现有 CHANGELOG.md 使用了不同格式，保持其格式
- 没有 tag 的仓库从第一个提交开始生成
- 合并提交（merge commit）自动跳过，不重复计数
