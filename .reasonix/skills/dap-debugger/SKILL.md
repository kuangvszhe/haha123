---
name: dap-debugger
description: DAP 调试器辅助 — 源自 omp 的 lldb/dlv/debugpy 调试工作流
---

# DAP 调试器辅助

## 来源
源自 Oh My Pi (omp) 的 28 个 DAP 调试操作 — lldb (C/C++/Rust)、dlv (Go)、debugpy (Python)。

## 用法
当需要调试程序崩溃、段错误、逻辑错误时使用。通过 bash 调系统调试器。

## 调试器选择

| 语言 | 调试器 | 命令 |
|---|---|---|
| C/C++/Rust | lldb | `lldb <binary>` |
| Go | dlv | `dlv debug` 或 `dlv attach <pid>` |
| Python | debugpy | `python -m debugpy --listen 5678` |
| 通用 | gdb | `gdb <binary>` |
| Windows | WinDbg/CDB | `cdb <command>` |

## 工作流

### 阶段 1: 确认调试器可用
```bash
# 检查可用的调试器
lldb --version 2>/dev/null && echo "lldb OK"
dlv version 2>/dev/null && echo "dlv OK"
python -c "import debugpy; print('debugpy OK')" 2>/dev/null
gdb --version 2>/dev/null && echo "gdb OK"
```

### 阶段 2: 编译带调试符号
```bash
# Rust
cargo build

# C/C++
gcc -g -o program program.c

# Go
go build -gcflags='-N -l' -o program .
```

### 阶段 3: 用 lldb 调试段错误
```bash
# 运行程序，捕获崩溃
lldb -o "run" -o "bt" -o "frame variable" -o "quit" ./program

# 或交互式
lldb ./program
# (lldb) run
# (lldb) bt
# (lldb) frame variable
# (lldb) quit
```

### 阶段 4: 用 dlv 调试 Go
```bash
# 启动调试
dlv debug

# 非交互式（批量命令）
dlv debug -- --headless --listen=:2345 --api-version=2 --accept-multiclient
# 然后用脚本发送命令
```

### 阶段 5: 用 debugpy 调试 Python
```bash
# 在代码中插入
# import debugpy
# debugpy.listen(5678)
# debugpy.wait_for_client()

# 或直接启动
python -m debugpy --listen 5678 --wait-for-client script.py
```

## 约束
- 调试器需要系统安装，不是所有环境都有
- 非交互式调试能力有限——复杂的逐步调试建议在终端中手动进行
- 调试完成后移除所有调试代码和断点
