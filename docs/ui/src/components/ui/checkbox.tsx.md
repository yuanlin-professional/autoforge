# `checkbox.tsx` -- 复选框组件

> 源文件路径: `ui/src/components/ui/checkbox.tsx`

## 功能概述

基于 `@radix-ui/react-checkbox` 原语封装的复选框组件，提供无障碍的受控/非受控复选框功能。选中时显示 Lucide CheckIcon 图标，支持暗色模式和禁用状态。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-checkbox` | Radix UI 复选框原语 |
| `lucide-react` | `CheckIcon` 选中图标 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `ScheduleModal.tsx` | `Checkbox` |

## 组件 Props / Variants

### `Checkbox`

- **Props**: `React.ComponentProps<typeof CheckboxPrimitive.Root>`（继承 Radix Checkbox 全部 Props）
- **无额外变体**，单一样式
- **样式特性**:
  - 尺寸: `size-4` (16px)
  - 圆角: `rounded-[4px]`
  - 未选中: 边框色 `border-input`，暗色模式下半透明背景
  - 选中: 主色背景 (`bg-primary`)，主色前景文字
  - 聚焦: 边框环 + 3px ring
  - 禁用: 降低透明度 + 禁止点击
  - 无障碍: 支持 `aria-invalid` 错误状态样式

### 内部结构

- `CheckboxPrimitive.Root` -- 复选框根元素
- `CheckboxPrimitive.Indicator` -- 选中状态指示器，包裹 `CheckIcon`（3.5 尺寸）

## 注意事项

- 通过 Radix 原语提供完整的无障碍支持（键盘导航、屏幕阅读器）
- 使用 `peer` 类名，支持与相邻 `Label` 组件联动样式
- 当前仅在 `ScheduleModal.tsx` 中使用
