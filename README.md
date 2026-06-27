# Reasonix — 集各家之长

> 融合 Claude Code、OpenCode、CodeX、OpenClaw、Oh My Pi (omp)、gstack、Google Design.md、Trae CN 等所有顶级 AI 编程代理的最佳实践。

## 📦 已集成能力

| 类型 | 数量 | 详情 |
|---|---|---|
| 📜 宪法 | 1 | `AGENTS.md` — 行为准则、工作流、约定 |
| 🧩 Skills | 10 | 从 Git 到安全审计的完整工作流 |
| 🔧 MCP | 2 | GitHub (26 工具) + Playwright (23 工具) |
| 🧠 记忆 | 4 | 跨会话持久化 |

## 🚀 快速开始

```bash
# 查看所有可用 Skill
ls .reasonix/skills/

# 使用 Skill（在 AI 对话中）
# run_skill({name: "office-hours"})
# /arch-review
```

## 📖 核心文件

| 文件 | 说明 |
|---|---|
| `AGENTS.md` | 项目宪法 — AI 代理必须读取 |
| `.reasonix/skills/*/SKILL.md` | 各个 Skill 的详细用法 |
| `.reasonix/memory/` | 跨会话记忆（持久化） |

## 🧩 Skill 清单

| Skill | 来源 | 功能 |
|---|---|---|
| `git-workflow` | Claude Code | Git 工作流自动化 |
| `debug-helper` | Oh My Pi | 系统化调试 |
| `code-review` | Oh My Pi + CodeX | 代码审查清单 |
| `multi-agent-orchestra` | CodeX + Oh My Pi | 多代理编排 |
| `dap-debugger` | Oh My Pi | DAP 调试器辅助 |
| `office-hours` | gstack | 产品思维追问 |
| `arch-review` | gstack + omp | 架构审查 |
| `investigate` | gstack + omp | 根因调查 |
| `security-audit` | gstack + OpenClaw | 安全审计 |
| `design-constraint` | Google Design.md | 设计规范约束 |
| `browser-automation` | Ava + Playwright MCP | 浏览器控制(23工具) |

---

*由 Reasonix 自动维护 · 集各家之长*
