# `switch.tsx` -- 开关组件

> 源文件路径: `ui/src/components/ui/switch.tsx`

## 功能概述

基于 `@radix-ui/react-switch` 原语封装的切换开关组件，提供两种尺寸变体。支持受控/非受控模式，具备完整的无障碍支持。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-switch` | Radix UI 开关原语 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `SettingsModal.tsx` | `Switch` |
| `HumanInputForm.tsx` | `Switch` |

## 组件 Props / Variants

### `Switch`

- **Props**: `React.ComponentProps<typeof SwitchPrimitive.Root>` + `{ size?: "sm" | "default" }`

**尺寸变体 (`size`):**

| 尺寸 | 轨道尺寸 | 手柄尺寸 |
|------|----------|----------|
| `default` | h-[1.15rem] w-8 | size-4 (16px) |
| `sm` | h-3.5 w-6 | size-3 (12px) |

**样式特性:**
- 轨道:
  - 选中: `bg-primary`
  - 未选中: `bg-input`，暗色模式 `bg-input/80`
  - 全圆角 (`rounded-full`)
  - 聚焦: 边框环 + 3px ring
  - 禁用: 降低透明度 + 禁止点击
- 手柄 (`Thumb`):
  - 圆形 (`rounded-full`)
  - 选中时平移 `translate-x-[calc(100%-2px)]`
  - 暗色模式未选中时使用前景色背景，选中时使用主色前景

### 内部结构

- `SwitchPrimitive.Root` -- 开关根元素（轨道）
- `SwitchPrimitive.Thumb` -- 滑动手柄

## 注意事项

- 文件顶部包含 `"use client"` 指令
- 使用 `data-size` 属性控制尺寸，配合 `data-[size=*]` 选择器实现尺寸变体
- 使用 Tailwind `group/switch` 命名分组，手柄通过 `group-data-[size=*]/switch` 响应尺寸变化
- 通过 `peer` 类名支持与相邻 `Label` 组件的联动
- 当前在 2 个组件中使用（设置面板和人工输入表单）
