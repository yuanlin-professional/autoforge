# `dropdown-menu.tsx` -- 下拉菜单组件

> 源文件路径: `ui/src/components/ui/dropdown-menu.tsx`

## 功能概述

基于 `@radix-ui/react-dropdown-menu` 原语封装的完整下拉菜单组件系列，支持普通菜单项、复选框菜单项、单选菜单项、子菜单、分组、标签、分隔线、快捷键提示等全部菜单交互模式。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-dropdown-menu` | Radix UI 下拉菜单原语 |
| `lucide-react` | `CheckIcon`（复选框指示器）、`ChevronRightIcon`（子菜单箭头）、`CircleIcon`（单选指示器） |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `ProjectSelector.tsx` | `DropdownMenu`, `DropdownMenuContent`, `DropdownMenuGroup`, `DropdownMenuLabel`, `DropdownMenuItem`, `DropdownMenuSeparator`, `DropdownMenuTrigger` |

## 组件 Props / Variants

### 容器组件

| 组件 | Radix 原语 | 说明 |
|------|-----------|------|
| `DropdownMenu` | `Root` | 菜单根容器 |
| `DropdownMenuPortal` | `Portal` | Portal 容器 |
| `DropdownMenuTrigger` | `Trigger` | 触发器 |
| `DropdownMenuGroup` | `Group` | 菜单项分组 |
| `DropdownMenuRadioGroup` | `RadioGroup` | 单选分组 |
| `DropdownMenuSub` | `Sub` | 子菜单容器 |

### `DropdownMenuContent`

菜单内容面板。Props: `sideOffset?: number`（默认 4）。

样式：圆角 `rounded-md`、边框、弹出层背景色、1px 内边距、阴影。自动渲染在 Portal 中。带方向感知的进出动画（从顶部/底部/左侧/右侧滑入）。

### `DropdownMenuItem`

普通菜单项。

- **额外 Props**: `inset?: boolean`（缩进模式）、`variant?: "default" | "destructive"`
- 聚焦时显示强调色背景
- `destructive` 变体显示红色文字和 SVG 图标

### `DropdownMenuCheckboxItem`

复选框菜单项。选中时左侧显示 `CheckIcon`。

### `DropdownMenuRadioItem`

单选菜单项。选中时左侧显示填充圆点 `CircleIcon`。

### `DropdownMenuLabel`

菜单标签。小号加粗文字，支持 `inset` 缩进。

### `DropdownMenuSeparator`

菜单分隔线。1px 高度，边框色背景。

### `DropdownMenuShortcut`

快捷键提示。右对齐，柔和前景色，超小字号。

### `DropdownMenuSubTrigger`

子菜单触发器。右侧显示 `ChevronRightIcon` 箭头。支持 `inset` 缩进。

### `DropdownMenuSubContent`

子菜单内容面板。与 `DropdownMenuContent` 类似样式，但使用 `shadow-lg`。

## 注意事项

- 通过 Radix 原语提供完整的无障碍支持（键盘导航、ARIA 角色、焦点管理）
- 所有子组件通过 `data-slot` 属性标记
- 当前仅在 `ProjectSelector.tsx` 中使用
- 导出 16 个子组件，覆盖下拉菜单的全部使用场景
