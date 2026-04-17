# Create JARVIS Skill

**Turn any AI agent into your product's expert — in one session.**

JARVIS is an agent-first product knowledge base methodology. Give this skill to your AI agent (Claude Code, OpenClaw, Codex, Cursor, GitHub Copilot, or any other), and it will build a structured "brain" for your product — so every agent on your team deeply understands your codebase, bug history, design decisions, and engineering context.

## The Problem

AI agents are smart but amnesiac. Every session starts from zero. They don't know:
- Why your system was designed this way
- Which bugs keep coming back and why
- What features were already rejected (and the reasoning)
- How your modules interact under the hood

You end up repeating context, and the agent keeps making suggestions your team already tried and discarded.

## What JARVIS Gives You

A **living knowledge repository** (a git repo) that any AI agent can read to instantly become a product expert:

```
<your-product>-jarvis/
├── README.md                    # Module quick-reference
├── MAINTENANCE.md               # Data sources, update rules, workflows
├── modules/
│   └── <module>/
│       ├── overview.md          # What it does, key code paths
│       ├── known-issues.md      # Bug pattern index (not raw tickets)
│       ├── decisions.md         # Why the system is the way it is
│       ├── rejected-features.md # What was tried and why it was rejected
│       └── test-coverage.md     # What's tested, what's not
└── cross-cutting/
    ├── module-interactions.md   # How modules depend on each other
    └── version-changelog.md     # Release history index
```

### The Three-Layer Architecture

| Layer | What it stores | Why it matters |
|-------|---------------|----------------|
| **History** | Bug patterns, design decisions, rejected features | Prevents repeating mistakes. This is the most valuable layer. |
| **Present** | Current backlog, version plans, team structure | Gives agents situational awareness. |
| **Future** | AI-generated analysis: dedup detection, root cause, impact | Agents produce insights, not just retrieve facts. |

**Priority: History >> Present > Future.** The quality of the History layer directly determines how good your agent's judgment is.

## How to Use

### Step 1: Open SKILL.md with your agent

Give your AI agent the [SKILL.md](./SKILL.md) file. That's the instruction set.

Examples:
- **Claude Code**: `Read SKILL.md and help me build JARVIS for my product`
- **OpenClaw/Hermes**: Load as a skill or paste the content
- **Codex/Cursor**: Paste SKILL.md content into the conversation
- **Any agent**: Just share the file — if it can read markdown, it can run JARVIS

### Step 2: Answer the agent's questions

The agent will ask about your product: what it does, where the code lives, where docs and issues are tracked, etc.

### Step 3: Let it connect and scan

The agent connects to your data sources (GitHub/GitLab, Jira/Linear, Confluence/Notion, etc.), then systematically scans your codebase, issues, and docs to build the knowledge base.

### Step 4: Get your JARVIS repo

You get a complete `<product>-jarvis` git repository, ready to use. Point any agent at it and they'll have deep product context from day one.

## After Setup: The Closed Loop

JARVIS isn't a one-time export. It stays alive through a simple workflow:

```
START  →  Query JARVIS for context before any task
WORK   →  Do the work (fix bug, build feature, investigate issue)
END    →  Write back what you learned to JARVIS
```

Every bug fix, every design decision, every rejected proposal gets fed back. The knowledge compounds.

## Background Reading

These articles explain the methodology and philosophy behind JARVIS:

1. [Building JARVIS for Software Companies](https://chenjunhao.cn/2026/03/23/building-jarvis-for-software-companies/)
2. [Why One-Shot AI Dev Experiments Can't Reveal the Real Upper Bound](https://chenjunhao.cn/2026/03/24/why-one-shot-ai-dev-experiments-cannot-reveal-the-real-upper-bound/)
3. [From AI Writing Code to AI-Driven R&D — The Gap Is JARVIS](https://chenjunhao.cn/2026/03/24/from-ai-writing-code-to-ai-driven-r-and-d-the-gap-is-jarvis/)
4. [How to Run an AI R&D Experiment: What You Validate Is Loop Closure, Not Code Output](https://chenjunhao.cn/2026/03/24/how-to-run-an-ai-r-and-d-experiment-what-you-validate-is-loop-closure-not-code-output/)
5. [JARVIS and Harness Engineering](https://chenjunhao.cn/2026/03/31/jarvis-and-harness-engineering/)

## License

© 2026 [Hengshi](https://github.com/hengshi). All rights reserved.

JARVIS is a paid consulting product. This skill is provided for licensed users to bootstrap their JARVIS knowledge base. For licensing and consulting inquiries, contact us via [chenjunhao.cn](https://chenjunhao.cn).
