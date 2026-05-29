---
name: git-helper
description: Git 操作助手：生成 commit message、写 PR 描述、分析历史、解决冲突。
---
# git-helper

辅助日常 Git 操作，减少手动输入和提高提交质量。

## 操作

### commit（生成提交信息）
1. 运行 `git diff --staged` 或 `git diff` 查看变更
2. 分析改动内容，生成符合 Conventional Commits 规范的提交信息：
   ```
   <type>(<scope>): <简短描述>

   <详细说明（可选）>
   ```
   type: feat / fix / refactor / docs / test / chore / style / perf
3. 展示建议的 commit message，让用户确认后执行 `git commit -m "..."`

### pr（生成 PR 描述）
1. 运行 `git log main..HEAD --oneline` 获取当前分支的提交
2. 生成 PR 描述模板：
   ```markdown
   ## 变更摘要
   [一句话]

   ## 详细说明
   - 变更 1
   - 变更 2

   ## 测试
   - [ ] 单元测试通过
   - [ ] 手动测试步骤

   ## 截图（如有 UI 变更）
   ```

### log（分析提交历史）
1. 运行 `git log --oneline -n <数量>` 获取最近提交
2. 按类型分组统计（feat 几个、fix 几个、refactor 几个）
3. 标注可疑提交（太大的 diff、没有描述的提交）

### conflict（解决冲突指导）
1. 运行 `git diff --name-only --diff-filter=U` 列出冲突文件
2. 逐个读取冲突文件，分析冲突原因
3. 给出解决建议（保留哪一侧、合并策略）

## 规则
- **绝不自动执行 `git push`**——始终让用户手动确认
- 不修改 `.git/` 目录下的任何文件
- commit message 长度 ≤ 72 字符（首行）
- 中文项目用中文提交信息，英文项目用英文
- 不 amend 已经 push 的提交
