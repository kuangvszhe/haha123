---
name: sandbox-exec
description: 容器沙箱执行 — 源自 Hermes Agent 沙箱系统
---

# 容器沙箱执行

## 来源
源自 Hermes Agent 的容器沙箱系统（Docker/Singularity/Modal/Daytona 4种后端）。用于安全隔离的高危操作执行。

## 用法
当需要执行高危操作（删除文件、运行不可信代码、操作生产环境、批量修改）时，自动在沙箱中执行。

## 沙箱模式
| 可用后端 | 何时使用 |
|---|---|
| **Docker** | 最通用，本地有 Docker 时首选 |
| **无沙箱** | 没有容器环境时，fallback 到 git stash + 警告 |

## 高危操作清单
以下操作必须先创建沙箱/快照再执行：
- `rm -rf` / 批量删除
- 修改数据库
- 运行外部下载的脚本
- 执行 curl | bash
- 修改系统配置
- 批量替换（sed -i、rename）

## 流程
```
发现高危操作
  → 检查是否有 Docker（docker --version）
  → 有: 在 Docker 容器中执行
  → 无: 创建 git checkpoint 再执行
  → 执行完成后对比结果
  → 确认无误后合并变更
```

## 约束
- Docker 不是强制依赖——没有就 fallback 到 checkpoint
- 沙箱隔离不是绝对安全，仅减少风险
- 沙箱内无法访问局域网资源
