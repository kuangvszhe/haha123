# Reasonix — 集各家之长

> 融合 Claude Code、Oh My Pi (omp)、OpenAI CodeX、gstack、Google Design.md、Ava、Hermes Agent、Trae CN 等所有顶级 AI 编程代理的最佳实践。
> 每个工具的独有优势已通过 Skills + MCP + AGENTS.md 宪法吸收整合。

## 📦 已集成能力

| 类型 | 数量 | 详情 |
|---|---|---|
| 📜 宪法 | 1 | `AGENTS.md` — 167行，行为准则+工作流+自动触发规则+决策矩阵 |
| 🧩 Skills | **21** | 覆盖 规划→检查→创作→修改→审查→交付 全6环节 |
| 🔧 MCP | **3** | GitHub (26) + Playwright (23) + 知识图谱记忆 (9) |
| 🧠 记忆 | **3层** | 文件记忆 + AGENTS.md规则 + 知识图谱(SQLite) |

## 来源总表

| 来源 | 贡献的技能 |
|---|---|
| **Claude Code** | git-workflow, hooks-auto-check, ci-cd-review, cross-file-analysis, semantic-memory, auto-compact |
| **Oh My Pi (omp)** | debug-helper, dap-debugger, code-review |
| **CodeX** | multi-agent-orchestra |
| **gstack** | office-hours, arch-review, investigate, security-audit |
| **Google Design.md** | design-constraint |
| **Ava** | browser-automation (Playwright MCP) |
| **Hermes Agent** | loop-guard, checkpoint, sandbox-exec, curator-memory, self-evolve |
| **Trae CN** | 中文母语支持 |

## 🚀 快速开始

```bash
# 查看所有可用 Skill（21个）
ls .reasonix/skills/

# 使用 Skill
# run_skill({name: "office-hours"})
# /security-audit
```

## 🧩 21 Skills 清单

### 规划
| Skill | 来源 | 功能 |
|---|---|---|
| `office-hours` | gstack | YC级产品思维追问（6步追问法） |
| `arch-review` | gstack + omp | 架构审查：数据流/状态机/错误矩阵 |

### 检查
| Skill | 来源 | 功能 |
|---|---|---|
| `hooks-auto-check` | Claude Code | 编辑后自动format→lint→type-check→test |
| `cross-file-analysis` | Claude Code | 5层分析法实现代码图级理解 |
| `auto-compact` | Claude Code | 上下文窗口超过80%时自动压缩 |

### 创作
| Skill | 来源 | 功能 |
|---|---|---|
| `design-constraint` | Google Design.md | 设计令牌+组件规范约束 |

### 修改
| Skill | 来源 | 功能 |
|---|---|---|
| `git-workflow` | Claude Code | Git提交/分支/回滚标准化 |
| `debug-helper` | Oh My Pi | 5阶段系统化调试工作流 |
| `dap-debugger` | Oh My Pi | lldb/dlv/debugpy系统调试器 |
| `investigate` | gstack + omp | 根因调查+事后分析模板 |
| `sandbox-exec` | Hermes | 高危操作容器沙箱执行 |

### 审查
| Skill | 来源 | 功能 |
|---|---|---|
| `code-review` | Oh My Pi + CodeX | 审查清单+优先级评分 |
| `security-audit` | gstack + OpenClaw | OWASP Top10 + STRIDE威胁建模 |
| `ci-cd-review` | Claude Code | GitHub Actions 4阶段自动审查 |

### 交付
| Skill | 来源 | 功能 |
|---|---|---|
| `browser-automation` | Ava | 浏览器控制(导航/点击/截图/调试) |
| `multi-agent-orchestra` | CodeX + Oh My Pi | 并行扇出/流水线/主从编排 |

### 基础设施
| Skill | 来源 | 功能 |
|---|---|---|
| `loop-guard` | Hermes | 工具循环防护（死循环/重复失败检测） |
| `checkpoint` | Hermes | 文件级快照回滚 |
| `curator-memory` | Hermes | 自动记忆整理(30天归档/90天删除) |
| `semantic-memory` | Claude Code | 语义记忆搜索（AI找相关记忆） |
| `self-evolve` | Hermes | 自进化：根据使用自动优化 |

## 🔧 MCP 工具

| 服务器 | 工具数 | 能力 |
|---|---|---|
| GitHub | 26 | issues/PRs/repos/code搜索/文件管理 |
| Playwright | 23 | 浏览器自动化(导航/点击/截图/调试/网络) |
| Knowledge Graph | 9 | 语义搜索+实体关系+持久化存储 |

## 🧠 记忆系统（3层架构）

```
层1: 文件记忆 → remember/forget工具，.md文件持久化
层2: 规则记忆 → AGENTS.md定时+curator-memory+semantic-memory
层3: 知识图谱 → SQLite实体-关系图，语义搜索
```

## 🤖 自动触发规则

遇到以下场景，Skills 自动触发无需手动调用：

| 场景 | 触发 |
|---|---|
| 新功能需求 | office-hours + arch-review |
| 编辑文件后 | hooks-auto-check (format→lint→type→test) |
| 提交/PR | git-workflow |
| 涉及安全 | code-review 安全清单 → security-audit |
| 前端问题 | browser-automation (打开浏览器查看) |
| 上下文快满 | auto-compact (自动压缩) |
| 死循环 | loop-guard (检测+停止) |
| 大变更前 | checkpoint (快照) |
| 任务完成 | self-evolve (优化) |

---

*由 Reasonix 自动维护 · 集各家之长 · 21 Skills + 3 MCP + 知识图谱记忆*
