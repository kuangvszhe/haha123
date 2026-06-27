---
name: checkpoint
description: 文件级快照回滚 — 源自 Hermes Agent Checkpoints
---

# Checkpoint — 文件级快照回滚

## 来源
源自 Hermes Agent 的 Checkpoints 系统。在重大变更前自动保存快照，支持文件级回滚。

## 用法
在进行重大重构、批量编辑、或危险操作前自动调用。也可以手动 `run_skill checkpoint` 触发。

## 流程

### 创建快照
```bash
# 在重大变动前自动执行：
1. git stash 或 git add + git stash 保存当前工作区
2. 记录快照时间戳和变更内容描述
3. 保存到 .reasonix/checkpoints/ 目录
```

### 列出快照
```bash
# 查看可用的回滚点
ls .reasonix/checkpoints/
```

### 回滚快照
```bash
# 恢复到指定快照
git stash pop  # 恢复
# 或
git checkout <checkpoint-hash>
```

## 触发场景
| 场景 | 自动触发 |
|---|---|
| 批量修改超过3个文件 | ✅ 自动创建快照 |
| 删除文件 | ✅ 自动创建快照 |
| 大规模重构 | ✅ 自动创建快照 |
| 修改配置文件 | ✅ 自动创建快照 |
| 单文件编辑 | ❌ 不需要 |

## 约束
- 快照仅保留最近20个，自动清理旧的
- 仅跟踪 git 管理的文件
- 不存储敏感信息（密钥、密码）
