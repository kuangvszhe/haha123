---
name: loop-guard
description: 工具循环防护 — 源自 Hermes Agent tool_loop_guardrails
---

# Tool Loop Guardrails — 工具循环防护

## 来源
源自 Hermes Agent 的 tool_loop_guardrails 系统。防止 AI agent 陷入死循环。

## 用法
自动运行。当连续出现相同错误、同一工具反复失败、或产生无进展重试时触发。

## 检测规则

### 级别 1: 警告（Warn After）
| 模式 | 触发阈值 | 行为 |
|---|---|---|
| **exact_failure** | 同一错误2次 | 记录警告日志，尝试换方法 |
| **same_tool_failure** | 同一工具失败3次 | 记录警告，标记该工具有问题 |
| **idempotent_no_progress** | 无进展重试2次 | 记录警告，建议换个角度 |

### 级别 2: 硬停止（Hard Stop After）
| 模式 | 触发阈值 | 行为 |
|---|---|---|
| **exact_failure** | 同一错误5次 | 停止当前操作，ask用户 |
| **same_tool_failure** | 同一工具失败8次 | 放弃该工具，换替代方案 |
| **idempotent_no_progress** | 无进展5次 | 重评估任务，ask用户 |

## 退出策略
当被硬停止时：
1. 记录当前已尝试的路径
2. 向用户总结：什么问题、尝试了什么、建议
3. 等用户指示后再继续

## 约束
- 只在连续重复失败时触发，不影响正常流程
- 如果用户明确说"继续试"，覆盖硬停止
- 不记录正常工具调用，只记录失败/无进展
