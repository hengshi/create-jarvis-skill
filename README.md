# Create JARVIS Skill

一个 agent-agnostic 的 skill，任何 AI agent 读完后都能帮用户从零构建一套 JARVIS 产品知识体系。

## 什么是 JARVIS

JARVIS 是 agent-first 的产品知识库方法论。它用三层时态架构（History/Present/Future）组织产品知识，让 AI agent 能深度理解产品、诊断问题、辅助工程决策。

## 使用方式

将 `SKILL.md` 发给你的 AI agent（OpenClaw、Claude Code、Codex、Cursor、GitHub Copilot 等），agent 会：

1. **Discovery** — 收集你的产品信息、代码仓库、文档来源
2. **Connect** — 建立与各数据来源的 CLI 连接和认证
3. **Scan & Build** — 用 JARVIS 方法论扫描数据来源，构建知识库
4. **Finalize** — 产出完整的 `<product>-jarvis` 仓库 + 维护指南

## 产出物

一个类似 [hengshi-jarvis](https://gitlab.hengshi.org/henglabs/hengshi-jarvis) 的仓库：

```
<product>-jarvis/
├── README.md
├── MAINTENANCE.md
├── modules/<module>/overview.md|known-issues.md|decisions.md|...
├── cross-cutting/
└── sources/
```

## 模板

`templates/` 目录下有各文件的模板供参考。
