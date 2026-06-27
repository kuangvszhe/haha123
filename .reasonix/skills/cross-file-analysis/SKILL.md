---
name: cross-file-analysis
description: 跨文件依赖分析 — 5层分析法实现代码图级理解
---

# 跨文件依赖分析

## 来源
当需要理解跨多个文件的代码关系、依赖链、数据流时使用。通过多工具组合实现类似 Claude Code 的代码图级理解。

## 五层分析法

### 第 1 层：文本搜索（快速定位）
```
grep({pattern: "<symbol>", path: "."})
```
找到所有提到目标符号的文件和位置。

### 第 2 层：符号追踪（LSP 精确跳转）
```
lsp_definition({file, line, symbol})   → 找到定义位置
lsp_references({file, line, symbol})   → 找到所有引用
lsp_hover({file, line, symbol})        → 查看类型签名
```
精准追踪接口/类型/函数的定义-引用关系。

### 第 3 层：子代理探索（上下文理解）
```
explore({task: "分析 X 模块的所有依赖关系和调用链"})
```
跨文件的深度分析，阅读多个源文件后给出完整报告。

### 第 4 层：批量并行读取（全貌概览）
```
parallel_tasks({
  tasks: [
    "读取 moduleA 的结构和导出的符号",
    "读取 moduleB 的结构和导出的符号",
    "分析 A 依赖 B 的完整路径"
  ]
})
```
同时理解多个模块的结构及其关系。

### 第 5 层：全量调查（复杂重构）
```
explore({task: "完整分析 X 功能涉及的所有文件、数据流、错误路径"})
```
适合重命名、重构、架构变更前的全量分析。

## 典型场景

### 场景：理解一个功能的完整调用链
```
1. grep("handleUserLogin")            → 找到入口
2. lsp_definition("handleUserLogin")  → 找到定义
3. lsp_references("handleUserLogin")  → 找到所有调用方
4. explore("追踪登录流程的所有文件")  → 完整分析
```

### 场景：重命名/重构前分析影响
```
1. grep("oldFunctionName")              → 所有引用
2. lsp_references("oldFunctionName")    → 精确引用列表
3. explore("重构风险评估")              → 影响分析
```

### 场景：理解新项目架构
```
1. explore("分析项目整体模块结构和依赖")
2. 或 parallel_tasks: 同时读取多个模块入口
```

## 约束
- 文本搜索（grep）可能产生误报（注释中的匹配）
- LSP 需要项目已安装语言服务器
- explore 是最深度的分析但消耗更多上下文
- 优先从 LSP 开始（精确），再 fallback 到 grep（全面）
