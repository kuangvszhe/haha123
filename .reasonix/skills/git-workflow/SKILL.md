---
name: git-workflow
description: Git 工作流自动化 — 源自 Claude Code 的最佳实践
---

# Git 工作流辅助

## 来源
源自 Claude Code 的 Git 工作流自动化最佳实践。

## 用法
当需要进行 Git 操作（提交、分支、合并、回滚等）时，按照以下步骤执行。

## 流程

### 1. 创建提交
```
1. git status — 查看当前变更
2. git diff — 查看具体变更内容（未暂存）
3. git diff --cached — 查看已暂存变更
4. 分析变更范围，生成有意义的提交信息（格式：type(scope): description）
5. git add <files> — 按逻辑分组暂存
6. git commit -m "type(scope): description"
```

### 2. 分支管理
```
1. git branch — 查看当前分支
2. git checkout -b <branch-name> — 创建新分支（命名规范：feat/ fix/ chore/ docs/ refactor/）
3. 工作完成后合并回主分支
```

### 3. 提交信息规范
```
类型: feat / fix / chore / docs / refactor / test / perf / style / ci
格式: <type>(<scope>): <简短描述>

示例:
- feat(auth): add OAuth2 login
- fix(parser): handle null input edge case
- chore(deps): upgrade lodash to 4.17.21
```

### 4. 回滚
```
1. git log --oneline -10 — 查看最近提交
2. git revert <commit-hash> — 安全回滚（生成新提交）
3. 仅在本地未推送时使用 git reset
```

## 约束
- 推送前必须经过 review 或测试验证
- 不要 commit 敏感信息（密钥、密码、token）
- 每次提交只做一件事（原子提交）
