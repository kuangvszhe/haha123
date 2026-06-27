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

*最后更新：2026-06-27*
*融合自：Claude Code, OpenCode, CodeX, OpenClaw, Oh My Pi (omp), Google Design.md, gstack, Trae CN, Reasonix*
*已集成 10 个 Skills + GitHub MCP + AGENTS.md 宪法*
