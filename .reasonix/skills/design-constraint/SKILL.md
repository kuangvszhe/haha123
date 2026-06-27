---
name: design-constraint
description: 设计规范约束 — 源自 Google Design.md 设计即代码
---

# 设计规范约束

## 来源
源自 Google Labs 的 [Design.md](https://github.com/google-labs-code/design.md) — 用结构化文件描述视觉设计体系。

## 用法
当生成 UI 代码、前端组件、或设计系统时，引用此 skill 来确保生成结果符合视觉规范。

## DESIGN.md 规范

### 必需的设计令牌

```yaml
# 色彩系统
colors:
  primary: "#..."        # 主色
  secondary: "#..."      # 辅助色
  background: "#..."     # 背景色
  surface: "#..."        # 表面色
  text: 
    primary: "#..."      # 主文字色
    secondary: "#..."    # 辅助文字色
    disabled: "#..."     # 禁用文字色
  error: "#..."          # 错误色
  success: "#..."        # 成功色
  warning: "#..."        # 警告色

# 排版
typography:
  fontFamily:            # 字体族
    primary: "..."
    mono: "..."
  fontSize:
    xs: "..."            # 极小
    sm: "..."            # 小
    base: "..."          # 基础
    lg: "..."            # 大
    xl: "..."            # 更大
    xxl: "..."           # 标题
  fontWeight:
    normal: 400
    medium: 500
    semibold: 600
    bold: 700

# 间距
spacing:
  xs: "..."              # 4px
  sm: "..."              # 8px
  md: "..."              # 16px
  lg: "..."              # 24px
  xl: "..."              # 32px

# 圆角
borderRadius:
  sm: "..."              # 4px
  md: "..."              # 8px
  lg: "..."              # 12px
  full: "..."            # 9999px

# 阴影
shadows:
  sm: "..."
  md: "..."
  lg: "..."

# 断点
breakpoints:
  sm: "640px"
  md: "768px"
  lg: "1024px"
  xl: "1280px"
```

### 组件规范
```yaml
components:
  Button:
    variants: [primary, secondary, ghost, danger]
    sizes: [sm, md, lg]
    states: [default, hover, active, disabled, loading]
  Input:
    variants: [outlined, filled]
    states: [default, focus, error, disabled]
  Card:
    variants: [elevated, outlined, flat]
```

## 约束
- 所有 UI 代码必须使用上述设计令牌
- 不用内联样式替代设计令牌
- 响应式设计使用断点系统
- 暗色模式需定义对应令牌
