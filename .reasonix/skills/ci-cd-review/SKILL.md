---
name: ci-cd-review
description: CI/CD 自动代码审查 — GitHub Actions 4阶段流水线
---

# CI/CD 自动代码审查

## 来源
源自 Claude Code 的 GitHub Actions CI 集成。对应 `.github/workflows/auto-code-review.yml`。

## 用法
当需要配置或触发 CI/CD 自动代码审查时使用。本 Skill 与 `.github/workflows/auto-code-review.yml` 配合工作。

## 流水线阶段

提交 PR 后，GitHub Actions 自动执行 4 阶段流水线：

### 阶段 1: 代码质量检查
根据项目语言自动检测并运行：
- Go: `go fmt` + `go vet`
- Rust: `cargo fmt` + `cargo clippy`
- Python: `ruff check`
- Node/TS: `npm run lint` + `tsc --noEmit`

### 阶段 2: 安全审计
- 依赖审计：`npm audit` / `pip-audit`
- 秘密扫描：TruffleHog

### 阶段 3: 测试
- 自动检测并运行项目测试框架
- Go: `go test`
- Rust: `cargo test`
- Python: `pytest`
- Node: `npm test`

### 阶段 4: AI 代码审查
- 获取 PR diff
- 自动生成审查评论
- 可集成 CodeRabbit / GitHub Copilot / 自定义 LLM API

## 约束
- 需要 GitHub Token 配置（自动由 GitHub Actions 提供）
- 仅在 GitHub 仓库中有效
- AI 审查阶段需额外配置 AI 服务接入
