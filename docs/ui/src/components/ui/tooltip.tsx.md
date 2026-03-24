# `tooltip.tsx` -- 工具提示组件

> 源文件路径: `ui/src/components/ui/tooltip.tsx`

## 功能概述

基于 `@radix-ui/react-tooltip` 原语封装的工具提示组件系列，提供带箭头的悬停提示信息。固定使用深色背景（`bg-neutral-900` + 白色文字），不受主题色影响，确保在所有主题下的可读性。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-tooltip` | Radix UI 工具提示原语 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `App.tsx` | `TooltipProvider`, `Tooltip`, `TooltipTrigger`, `TooltipContent` |
| `ThemeSelector.tsx` | `Tooltip`, `TooltipTrigger`, `TooltipContent` |

## 组件 Props / Variants

### `TooltipProvider`

提供者组件，包裹需要使用 Tooltip 的区域。

- **额外 Props**: `delayDuration?: number`（默认 250ms，悬停延迟）

### `Tooltip`

工具提示根容器。直接透传 Radix `TooltipPrimitive.Root` Props。

### `TooltipTrigger`

触发器组件，悬停时显示提示。

### `TooltipContent`

提示内容面板。

- **默认属性**:
  - `side`: `"bottom"`
  - `align`: `"center"`
  - `sideOffset`: `8`
- **样式特性**:
  - 背景: `bg-neutral-900`（深色，不随主题变化）
  - 文字: `text-white text-sm`
  - 圆角: `rounded-md`
  - 边框: `border`
  - 阴影: `shadow-md`
  - 最小高度: `min-h-7`
  - 行高: `leading-tight`
  - 进出动画: 方向感知的滑入/淡入
  - **箭头**: 自动渲染 `TooltipPrimitive.Arrow`，填充色 `fill-neutral-900` 与背景匹配

## 注意事项

- `TooltipProvider` 通常在应用顶层（`App.tsx`）包裹一次即可
- 内容面板固定深色背景，不跟随主题切换，确保在浅色和深色主题下都有良好对比度
- 箭头颜色与背景色统一为 `neutral-900`
- 默认显示在底部（`side="bottom"`），与触发元素间距 8px
- 通过 Portal 渲染到 DOM 树外部，避免被父级 `overflow: hidden` 裁切
