---
name: browser-automation
description: 浏览器自动化控制 — 源自 Ava CDP + Playwright MCP 23工具
---

# 浏览器自动化控制

## 来源
源自 Ava 的 Chrome CDP 浏览器控制 + Playwright MCP 23个工具。

## 用法
当需要调试 Web 前端、查看页面、操作浏览器时使用。通过 Playwright MCP 直接控制浏览器。

## 能力

### 页面导航与查看
- `browser_navigate` — 导航到任意 URL
- `browser_snapshot` — 获取页面的无障碍结构树（类 DOM 树）
- `browser_take_screenshot` — 截取当前页面（全页/可见区域/元素）
- `browser_console_messages` — 读取浏览器控制台日志（error/warning/info/debug）

### 页面交互
- `browser_click` — 点击任何元素
- `browser_type` — 输入文本到输入框
- `browser_select_option` — 选择下拉选项
- `browser_hover` — 悬停查看 tooltip/菜单
- `browser_fill_form` — 批量填写表单
- `browser_drag/drop` — 拖放操作
- `browser_file_upload` — 上传文件

### 调试工具
- `browser_network_requests` — 查看所有网络请求
- `browser_network_request` — 查看请求详情（headers/body）
- `browser_evaluate` — 在页面中执行 JS
- `browser_console_messages` — 提取前端日志
- `browser_resize` — 调整窗口尺寸模拟不同设备

### 多标签管理
- `browser_tabs` — list/new/close/select 多标签
- `browser_handle_dialog` — 处理 alert/confirm/prompt

## 标准调试流程
```
1. browser_navigate → 打开目标页面
2. browser_snapshot → 获取页面结构
3. browser_network_requests → 检查请求
4. browser_console_messages → 检查控制台错误
5. browser_take_screenshot → 截图验证
6. 定位并修复问题
```

## 约束
- 浏览器窗口需要可见（部分操作需要焦点）
- snapshot 返回的是无障碍树，不是 HTML 源码
- 截图是 PNG，可保存到本地
- 某些页面（如需要登录）需要先认证
