# [产品名] JARVIS 维护指南

> JARVIS 是 [产品名] 的 **索引 / 路由 / 汇总层**。
> 它不是内容副本仓库，是 agent 理解产品、定位问题、做出决策的大脑和导航系统。

---

## 1. 数据来源清单

| 来源 | 类型 | 访问方式 | 认证 |
|------|------|---------|------|
| `<org>/<repo>` | 代码仓库 | `gh` / `glab` CLI | `gh auth login` / `glab auth login` |
| Confluence | 文档 | `confluence-cli` | Token in `~/.confluence` |
| Jira | Issue 跟踪 | `jira-cli` | Token in `~/.jira` |
| ... | ... | ... | ... |

---

## 2. 三层时态架构

### History（最重要）— 已发生的事实

- `modules/*/known-issues.md` — 历史 bug 的模式索引（不是流水账）
- `modules/*/decisions.md` — 设计决策
- `modules/*/rejected-features.md` — 被否决需求
- `cross-cutting/` — 跨模块依赖、版本 changelog

### Present — 当前状态

- Backlog 快照、版本计划、团队配置

### Future — AI 判断产出

- 去重检测、根因分析、影响评估

**维护优先级：History >> Present > Future。**

---

## 3. Write Contracts

### 总原则

1. **索引，不是搬运** — 不搬 source 原文，保存可检索的组织层
2. **模式，不是流水** — known-issues 归纳模式，不逐条搬 issue
3. **同一知识只保留一个主落点**
4. **先读后写** — 追加任何文件前必须先读完整内容，匹配现有格式

### 各文件职责

| 文件 | 该写 | 不该写 |
|------|------|--------|
| `known-issues.md` | bug 模式、根因、修复策略 | 文档更新、feature 设计 |
| `decisions.md` | 设计决策 + 背景 + 选择原因 | 简单 bug 修复 |
| `rejected-features.md` | 被否决需求 + 否决原因 | 还在讨论中的需求 |
| `overview.md` | 模块职责、代码路径、核心概念 | 具体 bug 根因分析 |
| `faq.md` | 常见问题和对应逻辑解释 | 已修复且不再复现的 bug |

---

## 4. 更新触发条件

| 事件 | 更新什么 |
|------|---------|
| Bug 修复后 | `modules/<module>/known-issues.md` |
| Feature 完成后 | `modules/<module>/decisions.md` |
| 需求被否决后 | `modules/<module>/rejected-features.md` |
| 版本发布后 | `cross-cutting/version-changelog.md` |
| 新增测试后 | `modules/<module>/test-coverage.md` |

---

## 5. 工作流闭环：START → WORK → END

**START** — 查知识
- 读 JARVIS 中相关模块的 overview、known-issues、decisions
- 判断是否已知问题、是否有历史决策约束

**WORK** — 干活
- 按知识库指引定位代码、修复/实现
- 遵守各仓库的分支 + MR 规范

**END** — 回填知识
- 更新对应的 known-issues / decisions / rejected-features
- 确保下一个遇到类似问题的 agent 能直接从 JARVIS 找到答案
