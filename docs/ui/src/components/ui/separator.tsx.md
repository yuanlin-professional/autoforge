# `separator.tsx` -- 分隔线组件

> 源文件路径: `ui/src/components/ui/separator.tsx`

## 功能概述

基于 `@radix-ui/react-separator` 原语封装的分隔线组件，支持水平和垂直两种方向，提供无障碍语义（装饰性或语义性分隔）。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-separator` | Radix UI 分隔线原语 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `ScheduleModal.tsx` | `Separator` |
| `FeatureModal.tsx` | `Separator` |

## 组件 Props / Variants

### `Separator`

- **Props**: `React.ComponentProps<typeof SeparatorPrimitive.Root>`（继承 Radix Separator 全部 Props）
- **默认属性**:
  - `orientation`: `"horizontal"`（水平方向）
  - `decorative`: `true`（装饰性，对屏幕阅读器不可见）
- **样式特性**:
  - 背景色: `bg-border`
  - 水平: `h-px w-full`（1px 高度，撑满宽度）
  - 垂直: `w-px h-full`（1px 宽度，撑满高度）
  - `shrink-0` 防止在 flex 布局中被压缩

## 注意事项

- 文件顶部包含 `"use client"` 指令
- 当 `decorative` 为 `false` 时，Radix 原语会添加 `role="separator"` 无障碍属性
- 当前在 2 个组件中使用
