# AGENTS.md — 集各家之长

> 本文件融合了 Claude Code、OpenCode、CodeX(OpenAI)、OpenClaw、Oh My Pi(omp)、Trae CN、Reasonix 等顶级 AI 编程代理的最佳实践。
> 任何 AI 代理（planner / executor）在开始工作前必须读取此文件。

---

## 📋 项目核心规则

### 1. 三思而后行（源自 Claude Code / Reasonix）
- 在动手前先规划：使用 `todo_write` 列出子步骤
- 每完成一步用 `complete_step` 签字确认（附验证证据）
- 遇到模糊或重大决策时，用 `ask` 向用户提问（提供 2-4 个选项）

### 2. 最小变更原则（源自 Claude Code / CodeX）
- 改动必须最小且正确
- 优先使用 `edit_file` / `multi_edit` 做精确编辑，而非重写整个文件
- 删除用 `delete_range` / `delete_symbol` 保持干净

### 3. 验证驱动（源自 Oh My Pi）
- 每次修改后运行相关测试或编译检查（`lsp_diagnostics`）
- 不可验证的改动不提交
- 优先写测试再改代码（TDD 风格）

---

## 🔧 工具使用约定

### 代码搜索（源自 Oh My Pi 的 ripgrep 集成）
- 优先使用 `grep` / `glob` 而非 bash 命令搜索
- 符号查询用 `code_index` / `lsp_*` 系列工具
- 复杂架构问题用 `explore` 做全量调查

### LSP 集成（源自 OpenCode / omp）
- 编辑完后立即运行 `lsp_diagnostics` 检查编译错误
- 跳转定义用 `lsp_definition`
- 查看类型签名用 `lsp_hover`
- 查找引用用 `lsp_references`

### 子代理扇出（源自 Oh My Pi / CodeX）
- 独立子任务用 `task` 派发
- 可并行的任务用 `parallel_tasks` 同时执行
- 只读调研用 `read_only_task` / `explore`
- 每个子代理需给出明确的结果摘要

### 审查机制（源自 Oh My Pi 的顾问模型 / CodeX review）
- 重要改动完成后用 `review` 做代码审查
- 涉及安全（认证、输入、文件IO）的改动用 `security_review`
- 审查意见必须逐条处理或回复

### 记忆系统（源自 Claude Code CLAUDE.md / omp memory）
- 重要用户偏好用 `remember type=user` 持久化
- 工作反馈用 `remember type=feedback`（含 Why + How to apply）
- 项目约束用 `remember type=project`
- 外部资源链接用 `remember type=reference`
- 记忆过期/错误时用 `forget` 删除

---

## 🚀 工作流模板

### Git 工作流（源自 Claude Code）
```
1. git status / git diff 了解当前状态
2. todo_write 列出变更步骤
3. 逐步骤修改并验证
4. git add -p 交互式暂存（或逐个文件 add）
5. git commit -m 写有意义的提交信息
6. 提交完成后 review
```

### 调试工作流（源自 Oh My Pi + OpenClaw 沙箱）
```
1. 复现问题：bash 运行复现步骤
2. 缩小范围：grep / lsp 定位根因代码
3. 插入调试：提出修复方案
4. 应用修复：最小化改动
5. 验证：运行测试 / 编译检查
6. 清理：移除临时调试代码
```

### 安全敏感操作规范（源自 Claude Code 权限分级）
```
下列操作必须先 ask 用户确认：
- 删除文件/目录
- 修改 .env / 密钥文件
- 安装依赖/包
- 对外发送网络请求
- 修改系统配置
- 执行高权限命令
```

---

## 🧩 Skill 清单

以下 Skills 可通过 `run_skill({name})` 或 `/<name>` 调用：

| Skill | 功能 | 来源工具 |
|---|---|---|
| `git-workflow` | Git 提交/分支/回滚标准化 | Claude Code |
| `debug-helper` | 5 阶段调试工作流 | Oh My Pi |
| `code-review` | 完整审查清单 + 优先级评分 | Oh My Pi + CodeX |
| `multi-agent-orchestra` | 并行扇出/流水线/主从编排 | CodeX + Oh My Pi |
| `dap-debugger` | lldb/dlv/debugpy 系统调试器 | Oh My Pi (28 DAP ops) |
| `office-hours` | YC 级产品思维追问 | gstack |
| `arch-review` | 架构审查：数据流/状态机/错误矩阵 | gstack + omp |
| `investigate` | 根因调查 + 事后分析 | gstack + omp |
| `security-audit` | OWASP Top 10 + STRIDE 威胁建模 | gstack + OpenClaw |
| `design-constraint` | 设计令牌 + 组件规范约束 | Google Design.md |
| `hooks-auto-check` | 编辑后自动 format → lint → type-check → test | Claude Code Hooks |
| `ci-cd-review` | GitHub Actions 自动代码审查 + 安全扫描 | Claude Code CI |
| `cross-file-analysis` | 5 层分析法实现代码图级跨文件理解 | Claude Code |
| `browser-automation` | 浏览器控制：导航/点击/截图/调试/网络捕获 | Ava + Playwright MCP |
| `loop-guard` | 工具循环防护：死循环/重复失败/无进展自动检测 | Hermes Agent |
| `checkpoint` | 文件级快照回滚：大变前自动保存 | Hermes Agent |
| `sandbox-exec` | 容器沙箱执行：高危操作安全隔离 | Hermes Agent |
| `curator-memory` | 自动记忆整理：过期归档/压缩/清理 | Hermes Agent |
| `self-evolve` | 自进化：根据使用经验自动优化自身行为和技能 | Hermes Agent |
| `semantic-memory` | 语义记忆搜索：用 AI 找相关记忆而非关键词匹配 | Claude Code memdir |
| `auto-compact` | 自动上下文压缩：限时自动压缩历史保留关键信息 | Claude Code compact |

---

## 🏗️ 架构约定

- **模块化**：每个包/模块有清晰的职责边界
- **可测试**：核心逻辑与副作用分离（IO/网络/DB）
- **错误处理**：不吞错误，使用有意义的错误类型
- **文档**：公共 API 必须写 doc comment

---

## 🎯 行为准则

1. 用中文回答用户的提问（代码/术语保持原文）
2. 不确定时不猜测——用 `explore` 调研或用 `ask` 确认
3. 优先使用专用工具而非 shell 命令
4. 多步骤任务先规划再执行
5. 验证证据必须可复现

---

## 🧠 Skill 决策矩阵

以下矩阵解决了 Skill 之间的重叠和边界问题，避免"打架"。

### 调试三角：debug-helper vs dap-debugger vs investigate

| 场景 | 用哪个 | 为什么 |
|---|---|---|
| **普通 bug（逻辑错误/空指针/边界值）** | `debug-helper` | 5 阶段流程 + grep/LSP 定位，最快 |
| **段错误/崩溃/需要看内存/寄存器** | `dap-debugger` | 只有系统调试器能看堆栈和变量 |
| **生产故障/线上事故/来源不明** | `investigate` | 有时间线重建+假设验证+事后分析模板 |
| **先定位问题，再深入调试** | `debug-helper` → 发现问题在内存层面 → `dap-debugger` | 流程接替，从高层到低层 |

**规则：** 不知道根因时先用 `debug-helper`，需要看内存用 `dap-debugger`，线上事故用 `investigate`。

### 审查三角：code-review vs ci-cd-review vs security-audit

| 场景 | 用哪个 | 为什么 |
|---|---|---|
| **本地修改后的审查** | `code-review` | 人工审查清单 + 优先级评分 |
| **PR/CI 自动化审查** | `ci-cd-review` | 自动跑格式/lint/测试/AI审查 |
| **全量安全审计** | `security-audit` | OWASP + STRIDE + 秘密扫描 |
| **安全相关的代码改动** | `code-review` 的安全清单部分 → 需深入 → `security-audit` | 先快速扫，再深度审 |

**规则：** 安全相关改动先从 `code-review` 的安全清单开始，需深入才用 `security-audit`。`ci-cd-review` 是自动化的，和人工审查独立。

### hooks-auto-check vs ci-cd-review

| 触发时机 | 用哪个 | 覆盖范围 |
|---|---|---|
| **本地每次编辑文件后** | `hooks-auto-check` | format + lint + type-check（快速，<30秒） |
| **PR 推送到 GitHub 后** | `ci-cd-review` | 完整 4 阶段（含测试+安全+AI审查） |

**规则：** 两者互补不冲突。本地检查保证"提交前基本干净"，CI 检查保证"合并前全面过关"。本地通过了 CI 仍然会跑，因为 CI 环境更完整。

---

## 🤖 自动触发规则

以下规则定义了在特定场景下**我（AI 代理）会自动调用的 Skill**，不需要你每次手动要求。

### 开发流程自动触发

| 场景 | 自动调用的 Skill | 触发条件 |
|---|---|---|
| **开始新功能** | `office-hours` + `arch-review` | 当用户提出新的功能/产品需求时，先追问产品逻辑，再审查架构 |
| **修改代码后** | `hooks-auto-check` + `code-review` | 每次编辑文件后，自动运行 format → lint → type-check → test 四步检查，然后审查变更 |
| **提交/PR** | `git-workflow` | 新仓库首次 commit 使用标准格式 |
| **CI/CD 触发** | `ci-cd-review` | PR 推送后自动触发 4 阶段流水线（质量检查→安全审计→测试→AI审查） |
| **涉及安全** | 见审查三角决策矩阵 | `code-review`(安全清单) / `security-audit`(OWASP全量) / `ci-cd-review`(CI自动化) |
| **涉及调试** | 见调试三角决策矩阵 | `debug-helper`(普通bug) / `dap-debugger`(段错误) / `investigate`(生产故障) |
| **涉及设计** | `design-constraint` | 生成 UI 代码或前端组件时 |
| **生产故障** | `investigate` | 处理生产环境问题时 |
| **多任务并行** | `multi-agent-orchestra` | 需要同时做多个独立子任务时 |
| **跨文件追踪** | `cross-file-analysis` | 需要理解跨模块依赖、调用链、数据流时。自动从 grep 到 LSP 到 explore 逐层深入 |
| **Web 调试** | `browser-automation` | 调试 Web 前端、查看页面表现、检查网络请求或控制台错误时。自动打开浏览器查看 |
| **死循环检测** | `loop-guard` | 检测到连续重复失败/同一工具反复失败/无进展重试时自动触发 |
| **大变前快照** | `checkpoint` | 批量修改/删除/重构超过3个文件时自动创建快照 |
| **高危操作** | `sandbox-exec` | 检测到 rm -rf/批量替换/数据库修改等操作时自动调用 |
| **记忆过载** | `curator-memory` | 记忆数超过50条或任务完成时自动检查并整理 |
| **完成大任务** | `self-evolve` | 每次大型任务完成后自动回顾，根据信任等级（3次无投诉→全自动/否则 ask）优化 AGENTS.md/Skills/规则 |
| **上下文快满** | `auto-compact` | 检测到上下文使用率 > 80% 时自动触发压缩 |
| **需要旧信息** | `semantic-memory` | 用户问题可能涉及历史记忆时自动语义搜索 |

### 自动行为规则

- 写代码前 → 自动 `todo_write` 列步骤
- 每次 `edit_file` 后 → 自动 `lsp_diagnostics` 检查编译
- 完成一个阶段 → 自动 `complete_step` 记录证据
- 遇到模糊需求 → 自动 `ask` 而不是猜测

---

*最后更新：2026-06-27*
*融合自：Claude Code, OpenCode, CodeX, OpenClaw, Oh My Pi (omp), Google Design.md, gstack, Ava, Hermes Agent, Trae CN, Reasonix*
*已集成 21 个 Skills + 3 MCP (GitHub/Playwright/知识图谱) + AGENTS.md 宪法 — 全面超越单一工具*
*项目总览见 [README.md](./README.md)*
*浏览器控制已对齐 Ava，知识图谱记忆已对齐 claude-mem，自动记忆已对齐 Hermes*
*✅ 冲突审计已通过，决策矩阵就位，不打架*
