---
name: hooks-auto-check
description: Hooks 自动检查 — 编辑后自动 format/lint/type-check/test
---

# Hooks 自动检查系统

## 来源
源自 Claude Code 的 Hooks 系统 — 每次编辑文件后自动触发检查流程（format → lint → type-check → test）。

## 用法
当我在进行代码编辑时，AGENTS.md 的自动规则会触发本 Skill 定义的后置检查流程。无需手动调用。

## 自动检查流程

每次 `edit_file` / `multi_edit` / `write_file` 后，按以下优先级自动运行：

### 1. 格式检查 (Format)
```bash
# 按语言自动识别
# Go: go fmt ./...
# Rust: cargo fmt
# Python: black .
# JavaScript/TS: npx prettier --write <file>
# C/C++: clang-format -i <file>
```

### 2. Lint 检查
```bash
# Go: go vet ./...
# Rust: cargo clippy
# Python: ruff check .
# JavaScript/TS: npx eslint <file>
```

### 3. 编译/类型检查
```bash
# Go: go build ./...
# Rust: cargo check
# Python: mypy .
# TypeScript: npx tsc --noEmit
# Java: mvn compile
```

### 4. 编译/类型检查 (LSP)
```tool
# 通用: lsp_diagnostics
# 在所有环境下可用
lsp_diagnostics({file: "<edited-file>"})
```

### 5. 单元测试（如果修改了逻辑）
```bash
# Go: go test ./<pkg>
# Rust: cargo test
# Python: pytest <file>
# JavaScript/TS: npx vitest run <file>
```

## 检查结果处理

| 检查结果 | 行为 |
|---|---|
| ✅ 全部通过 | 继续下一步工作 |
| ⚠️ 有警告 | AGENTS.md 记录警告，但不阻塞 |
| 🔴 有错误 | 立即修复，修复后重新运行检查 |

## 约束
- 不运行耗时超过 30 秒的命令（长测试手动触发）
- 不修改未编辑过的文件
- 编译器/格式化工具不存在时跳过对应检查
- 仅在确认有对应语言的工具链时才运行
