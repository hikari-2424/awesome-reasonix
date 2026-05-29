# Reasonix 社区架构设计

> 长期规划——从一个人的 curation 到社区自治的演进路径。

---

## 一、整体架构

```
                        ┌──────────────────────────────────┐
                        │         awesome-reasonix         │
                        │      （中心索引 · GitHub Repo）    │
                        │                                  │
                        │  README.md   ← 人工 curation      │
                        │  skills/     ← 种子 skill 归档    │
                        │  registry.json ← 机器可读索引     │
                        └────────────┬─────────────────────┘
                                     │
              ┌──────────────────────┼──────────────────────┐
              │                      │                      │
    ┌─────────▼────────┐  ┌─────────▼────────┐  ┌─────────▼────────┐
    │  awesome-        │  │  GitHub          │  │  CLI 集成         │
    │  reasonix-site   │  │  Discussions     │  │                  │
    │  （可视化目录）    │  │  （讨论区）       │  │  reasonix skill  │
    │                  │  │                  │  │    search <q>    │
    │  搜索 · 浏览     │  │  Q&A · 经验分享   │  │  reasonix skill  │
    │  一键安装        │  │  公告 · RFC      │  │    install <id>  │
    └──────────────────┘  └──────────────────┘  └──────────────────┘
              │                      │                      │
              └──────────────────────┼──────────────────────┘
                                     │
    ┌────────────────────────────────┼──────────────────────────────┐
    │                        社区贡献者                              │
    │                                                              │
    │   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐  │
    │   │  Skill    │   │  MCP     │   │  工具    │   │  教程    │  │
    │   │  作者     │   │  作者    │   │  作者    │   │  作者    │  │
    │   └──────────┘   └──────────┘   └──────────┘   └──────────┘  │
    │         │               │               │              │      │
    │         └───────────────┴───────┬───────┴──────────────┘      │
    │                                 │                             │
    │                    各自维护独立 GitHub 仓库                    │
    │                    向 awesome-reasonix 提 PR                  │
    └──────────────────────────────────────────────────────────────┘
```

### 核心原则

**不建平台，建协议。** 

Claude Code 社区证明了：最成功的 AI 工具生态不需要中心化 marketplace。GitHub 就是分发层，Star 就是社交证明，PR 就是贡献通道。我们要做的是降低 friction，不是增加 friction。

---

## 二、四层架构详解

### 第 0 层：内容创作（最底层）

每个 Skill / MCP 服务器 / 工具就是一个**独立 GitHub 仓库**。

```
my-reasonix-skill/
├── README.md           # 简介、安装、使用示例
├── skill.md            # 核心 skill 定义文件
├── LICENSE
└── examples/           # 可选：使用截图、录屏
```

**为什么不用 monorepo？**
- 独立仓库 = 独立 Star / Issue / PR = 独立社区信号
- 作者有完全控制权，不需要等中心化 review
- 天然去中心化——没有人能删掉你的 skill
- Git 就是版本管理，不需要再造一个

### 第 1 层：中心索引（awesome-reasonix）

**角色**：策展层。不存内容，只存指针 + 评价。

```
awesome-reasonix/
├── README.md           # 人类可读的索引
├── CONTRIBUTING.md     # 收录标准
├── registry.json       # 机器可读的索引 ← 关键
├── skills/             # 种子 skill 归档（初期）
└── .github/
    ├── PULL_REQUEST_TEMPLATE.md
    └── workflows/
        └── validate.yml  # CI：自动校验 registry.json
```

**registry.json 设计**（机器可读索引）：

```json
{
  "version": "1",
  "updated": "2026-01-15T00:00:00Z",
  "entries": [
    {
      "id": "file-organizer",
      "type": "skill",
      "name": "file-organizer",
      "description": "按扩展名分类、去重扫描、目录树生成、空目录清理",
      "url": "https://github.com/hikari-2424/awesome-reasonix/blob/main/skills/file-organizer.md",
      "author": "reasonix",
      "tags": ["文件管理", "自动化"],
      "installCmd": "cp skills/file-organizer.md .reasonix/skills/",
      "curatedAt": "2026-01-10T00:00:00Z",
      "curatedBy": "reasonix"
    }
  ]
}
```

这个文件是 CLI 集成的基础——`reasonix skill search` 本质上就是 grep 这个 JSON。

### 第 2 层：可视化 & 分发（awesome-reasonix-site）

**角色**：降低发现成本。README 对机器友好但对人不友好——网站提供搜索、预览、一键复制。

当前用 Next.js（`awesome-reasonix-site`），可以考虑两个演进方向：

| 方向 | 适用场景 | 复杂度 |
|---|---|---|
| **静态站点**（当前方案） | 条目 < 200，手动 sync registry.json | 低 |
| **动态站点 + 定时同步** | 条目 > 500，需要实时更新 | 中 |
| **直接用 registry.json + 社区 CLI** | 重度 CLI 用户 | 低 |

**建议**：静态站足够用到条目破 200。这之前不需要数据库、不需要后端。Vercel 免费，GitHub 免费，维护成本为零。

### 第 3 层：讨论与治理（GitHub Discussions + RFC）

**角色**：社交层。不造论坛，用 GitHub Discussions。

原因：
- 贡献者已经在 GitHub 上——不需要「去另一个地方注册」
- 讨论和代码在同一个平台——「这个 skill 有个讨论」→ 链接直达
- 免费，零维护，支持分类、投票、置顶

**Discussions 分类设计：**

| 分类 | 用途 | 示例 |
|---|---|---|
| 📣 Announcements | curator 公告、生态周报 | "本周新收录 3 个 Skill" |
| 💡 Ideas | 新 skill / 工具提案 | "需要一个 Docker 管理 skill" |
| 🙏 Q&A | 使用问答 | "怎么让 skill 之间共享上下文？" |
| 📚 Show & Tell | 作品展示 | "我用 Reasonix 做了一个自动部署工具" |
| 🗳️ RFC | 重大决策讨论 | "要不要引入 skill 评分机制？" |

---

## 三、演进路线图

### 阶段 1：种子期（当前 → 50 条目）

**目标**：证明"有人来"。

| 动作 | 优先级 |
|---|---|
| ✅ 创建 awesome-reasonix 仓库 | P0 完成 |
| ✅ 创建可视化目录站 | P0 完成 |
| ⬜ 在 GitHub 上开启 Discussions | P0 |
| ⬜ 手动收录前 20-30 个条目（你创作 + 邀请朋友） | P0 |
| ⬜ 在社交圈（V2EX / 即刻 / Twitter）分享 | P1 |
| ⬜ 写一篇「如何创作一个 Reasonix Skill」教程 | P1 |

**不做的**：
- ❌ 不做用户系统
- ❌ 不做评分/评论
- ❌ 不做后端
- ❌ 不做 automated registry sync

**原因**：种子期最大的风险是「没人来」，不是「平台功能不够」。全部精力放在内容质量和推广上。

### 阶段 2：增长期（50 → 200 条目）

**目标**：完全自动化运转。

| 动作 | 优先级 |
|---|---|
| ✅ 自动化 Skill 审批（格式校验 + 安全扫描） | P0 完成 |
| ✅ 自动化内容审核（差评隐藏 + 举报检测） | P0 完成 |
| ✅ `quarantine.json` 隔离机制 | P0 完成 |
| ⬜ CLI 集成：`reasonix skill search` / `install` | P1 |
| ⬜ 按月发布「生态月报」（CI 自动生成） | P1 |

### 阶段 3：成熟期（200+ 条目）

**目标**：完全自我维持。

| 动作 | 优先级 |
|---|---|
| ⬜ 引入 community voting（GitHub Reactions） | P1 |
| ⬜ 分拆子目录（Skills 按领域拆分） | P1 |
| ⬜ 搜索和推荐算法优化 | P2 |

---

## 四、关键设计决策

### Q1：人工审核 vs 自动化？

**全自动化。** 所有 Skill 提交、评价、举报均由 CI 自动处理：

- **Skill 提交** → `auto-approve-skills.yml` 自动校验格式、安全扫描 → 即时发布或自动驳回
- **差评过多** → `auto-moderate-content.yml` 检测评分 < 2.0 且 3+ 评价 → 自动移入 `quarantine.json`
- **多次举报** → 同一 Skill 被举报 2+ 次 → 自动隔离
- **恶意内容** → 正则匹配危险命令模式 → 自动驳回

只有 `quarantine.json` 中标记 `status: pending-review` 的条目需要手动处理。

### Q2：要不要做用户系统 / 登录？

**GitHub OAuth 足够。** 站点使用 GitHub 登录进行身份认证，评价和举报通过 API 写入仓库。不需要自建用户系统。

### Q3：评分系统？

**已内置。** `reviews/` 目录存储用户评价，`ratings.json` 由 CI 自动聚合。评分公开透明，所有人可查看。

### Q4：如何保证可持续？

**零成本运行。** GitHub + Vercel 免费层完全足够。自动化 CI 处理所有审核工作，无需人工介入。

---

## 五、成功指标

| 阶段 | 指标 | 目标 |
|---|---|---|
| 种子期 | awesome-reasonix Star 数 | 100 |
| 种子期 | 外部贡献者 PR 数 | 10 |
| 增长期 | 收录条目数 | 100 |
| 增长期 | 自动化审批率 | > 95% |
| 成熟期 | 社区自发 PR 占比 | > 80% |

---

## 六、风险 & 应对

| 风险 | 概率 | 应对 |
|---|---|---|
| Reasonix 用户基数太小，没人贡献 | 高 | 丰富的种子内容降低入门门槛；全自动审批让贡献者即时看到结果 |
| 低质量条目涌入 | 中 | 自动化格式和安全检查拒绝明显不合格的提交；差评自动隐藏 |
| 恶意代码 | 低 | 安全扫描（危险命令检测）+ 社区举报 + 自动隔离 |
| 刷分作弊 | 中 | GitHub 账号关联（一个账号一票），异常模式可被检测 |
| 被 Claude Code 社区视为模仿 | 低 | awesome 列表是开源社区的标准模式，不存在「模仿」一说；注明 inspired by 即可 |
