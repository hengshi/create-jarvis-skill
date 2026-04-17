# Create JARVIS Skill

> **你是一个 AI agent，正在帮用户为他们的产品/公司构建 JARVIS 知识体系。**
> JARVIS 是一套让 AI agent 深度理解产品、诊断问题、辅助决策的知识库方法论。
> 读完这个 skill，你就能从零开始帮用户构建一套完整的 JARVIS。

---

## 什么是 JARVIS

JARVIS 是一个 **agent-first 的产品知识库**。它不是给人看的文档，是给 AI agent 用的"大脑"。

核心理念：
- **索引层，不是内容仓库** — JARVIS 存储的是路由、索引、模式总结，不是把文档搬过来
- **三层时态架构** — History（已发生的事实）、Present（当前状态）、Future（AI 判断产出）
- **闭环工作流** — 每个任务从 JARVIS 查知识开始，到回填 JARVIS 结束

### 三层时态架构

**History（最重要）**
- `modules/*/known-issues.md` — 历史 bug 的模式索引，用于根因分析
- `modules/*/decisions.md` — 设计决策，解释"为什么系统是这样"
- `modules/*/rejected-features.md` — 被否决的需求，防止反复提出已否决方案
- `cross-cutting/` — 跨模块依赖、版本 changelog

**Present**
- 当前 backlog 快照、版本计划、团队配置

**Future**
- 基于 History + Present 的 AI 判断：去重检测、根因分析、影响评估

**维护优先级：History >> Present > Future。** History 层质量直接决定 agent 判断能力。

---

## Phase 1 — Discovery（信息收集）

> 目标：搞清楚用户的产品全貌和数据来源。

向用户询问以下信息：

1. **产品基本信息**
   - 产品名称是什么？
   - 产品主要解决什么问题？面向什么用户？
   - 产品有哪些核心模块/功能领域？

2. **代码仓库**
   - 代码托管在哪里？（GitHub / GitLab / Bitbucket / 其他）
   - 有哪些仓库？分别是什么角色？（前端/后端/文档/测试/基础设施）
   - 每个仓库的默认分支是什么？

3. **文档来源**
   - 产品文档在哪里？（代码仓库内 / Confluence / Notion / 飞书 / 其他）
   - 有 API 文档吗？在哪里？

4. **Issue/任务系统**
   - 用什么跟踪 bug 和需求？（GitHub Issues / GitLab Issues / Jira / Linear / 飞书项目 / 其他）

5. **测试**
   - 有自动化测试吗？在哪个仓库？
   - 测试框架是什么？

6. **其他知识来源**
   - 有内部 wiki、设计文档、会议纪要等知识来源吗？
   - 有客户反馈渠道吗？（工单系统、客户群等）

收集完成后，确认你能通过 CLI 工具访问这些数据来源，进入 Phase 2。

---

## Phase 2 — Connect（工具链接与认证）

> 目标：确保你（agent）能从所有数据来源拉取内容。

根据 Phase 1 收集到的信息，逐个建立连接：

### 通用流程

对于每个数据来源：
1. 确认需要什么 CLI 工具（如 `gh`、`glab`、`jira`、`confluence-cli` 等）
2. 检查工具是否已安装，未安装则协助安装
3. 执行认证流程（如 `glab auth login`、配置 token 等）
4. **验证连接** — 执行一个简单的读取操作确认能拉到数据

### 验证清单

每个数据来源连通后，执行验证：
- [ ] 能列出仓库/项目
- [ ] 能读取 issue/任务
- [ ] 能读取代码文件
- [ ] 能读取文档内容

**全部数据来源连通后，进入 Phase 3。**

---

## Phase 3 — Scan & Build（扫描构建）

> 目标：扫描所有数据来源，按 JARVIS 方法论构建知识库。

### 3.1 创建仓库结构

在用户指定位置创建 JARVIS 仓库：

```
<product>-jarvis/
├── README.md                          # 仓库说明 + 模块速查
├── MAINTENANCE.md                     # 维护指南（含数据来源清单）
├── modules/                           # 按产品模块组织
│   └── <module-name>/
│       ├── overview.md                # 模块总览：职责、代码路径、关键概念
│       ├── known-issues.md            # 历史 bug 模式索引
│       ├── decisions.md               # 设计决策记录
│       ├── rejected-features.md       # 被否决需求
│       ├── faq.md                     # 常见问题
│       └── test-coverage.md           # 测试覆盖情况
├── cross-cutting/                     # 跨模块专题
│   ├── module-interactions.md         # 模块间依赖与交互
│   └── version-changelog.md          # 版本变更索引
└── sources/                           # 外部知识来源的路由
    └── <source-name>/
        └── README.md                  # 该来源的访问方式、索引
```

### 3.2 模块识别

从代码仓库结构和产品文档中识别核心模块：
- 扫描代码目录结构（前端路由、后端 API 目录、数据库 schema）
- 扫描文档目录结构
- 扫描 issue 标签/分类
- 综合以上信息，划分 10-20 个核心模块

### 3.3 逐模块构建

对每个模块，按以下顺序填充：

**Step 1 — overview.md**
- 从代码和文档中提取：模块职责、关键代码路径、核心概念、对外接口
- 使用 `templates/module-overview.md` 模板

**Step 2 — known-issues.md**
- 扫描 issue 系统中该模块相关的已关闭 bug
- 提取模式：重复出现的问题类型、典型根因
- 不要逐条搬运 issue，要总结成**模式索引**

**Step 3 — decisions.md**
- 从 MR 讨论、设计文档、代码注释中提取设计决策
- 格式：决策内容 + 背景 + 为什么这样选

**Step 4 — rejected-features.md**
- 从 issue 系统中找"won't fix"、"rejected"、"by design"的需求
- 记录被否决原因，这是最有价值的知识之一

**Step 5 — test-coverage.md**
- 扫描测试代码，记录该模块有哪些测试、覆盖什么场景

### 3.4 跨模块构建

- `module-interactions.md` — 从代码依赖和 issue 中提取模块间交互关系
- `version-changelog.md` — 从 release notes 或 git tag 中提取版本变更索引

### 3.5 并行策略

如果你支持子任务/subagent（如 OpenClaw 的 delegate_task、Claude Code 的 Task tool）：
- 多个模块的构建可以并行执行
- 每个子任务负责 1-3 个模块
- 主任务负责 cross-cutting 部分

如果不支持并行：
- 串行执行，按模块重要性排序（核心模块优先）

---

## Phase 4 — Finalize（完成交付）

> 目标：产出完整可用的 JARVIS 仓库。

### 4.1 编写 MAINTENANCE.md

这是 JARVIS 的运维手册，必须包含：

1. **数据来源清单** — 所有数据来源的访问方式（替代独立的 data-sources 文件）
   - 仓库地址、CLI 工具、认证方式
   - 文档系统、issue 系统的访问方式
2. **三层架构说明** — History/Present/Future 各层的内容和维护规则
3. **Write contracts** — 每类文件应该写什么、不该写什么
4. **更新触发条件** — 什么时候需要更新 JARVIS（bug 修复后、feature 完成后、版本发布后）
5. **工作流闭环** — START（查知识）→ WORK（干活）→ END（回填知识）

使用 `templates/maintenance.md` 模板。

### 4.2 编写 README.md

仓库首页，包含：
- 产品简介
- 模块速查表（模块名 + 说明 + 代码仓库映射）
- 快速使用指南（agent 读哪个文件开始）

### 4.3 创建 JARVIS Skill

为用户的 agent 创建一个类似 `hengshi-jarvis` 的 skill 文件，让 agent 日后能以此 skill 为入口使用 JARVIS。
skill 内容应包含：
- 任务路由（查询/工程/回填）
- 知识查询流程
- 工程工作流
- 知识库回填规则

### 4.4 初始化 Git 仓库

```bash
cd <product>-jarvis
git init
git add .
git commit -m "Initial JARVIS knowledge base"
# 推送到用户指定的远程仓库
```

### 4.5 验收清单

- [ ] 所有已识别模块都有 overview.md
- [ ] known-issues.md 包含从历史 issue 中提取的 bug 模式（至少核心模块有内容）
- [ ] decisions.md 包含关键设计决策
- [ ] MAINTENANCE.md 完整且包含数据来源清单
- [ ] README.md 包含模块速查表
- [ ] cross-cutting/ 下有模块交互和版本变更
- [ ] JARVIS skill 文件已创建

---

## 注意事项

1. **索引，不是搬运** — JARVIS 里不要复制粘贴大段原始文档，写摘要和指针
2. **模式，不是流水** — known-issues 要归纳模式，不要逐条搬 issue
3. **先读后写** — 追加任何文件前先读完整内容，匹配现有格式
4. **History 为王** — 时间有限时优先填充 History 层
5. **不完美也要交付** — 可以先覆盖核心模块，后续迭代补充
