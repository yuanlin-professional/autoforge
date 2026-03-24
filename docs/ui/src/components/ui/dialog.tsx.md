# `dialog.tsx` -- 对话框组件

> 源文件路径: `ui/src/components/ui/dialog.tsx`

## 功能概述

基于 `@radix-ui/react-dialog` 原语封装的模态对话框组件系列，提供完整的对话框布局（遮罩层、内容面板、头部、底部、标题、描述）。支持可配置的关闭按钮、动画进出效果，以及可选的底部关闭按钮。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-dialog` | Radix UI 对话框原语 |
| `lucide-react` | `XIcon` 关闭图标 |
| `@/lib/utils` | `cn` 类名合并工具 |
| `@/components/ui/button` | `Button` 组件（用于底部关闭按钮） |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `SettingsModal.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription` |
| `ResetProjectModal.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogFooter` |
| `KeyboardShortcutsHelp.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle` |
| `ScheduleModal.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogFooter` |
| `NewProjectModal.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription` |
| `FeatureModal.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle` |
| `DevServerConfigDialog.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogFooter` |
| `AddFeatureForm.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle` |
| `EditFeatureForm.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle` |
| `ConfirmDialog.tsx` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogFooter` |

## 组件 Props / Variants

### `Dialog`

对话框根组件，直接透传 Radix `DialogPrimitive.Root` Props。

### `DialogTrigger`

触发器组件，点击时打开对话框。

### `DialogPortal`

Portal 组件，将内容渲染到 DOM 树外部。

### `DialogClose`

关闭组件，点击时关闭对话框。

### `DialogOverlay`

遮罩层。半透明黑色背景 (`bg-black/50`)，z-index 50，带淡入/淡出动画。

### `DialogContent`

内容面板。核心属性：

- **额外 Props**: `showCloseButton?: boolean`（默认 `true`）
- 居中定位（`fixed top-[50%] left-[50%] translate`）
- 最大宽度: `sm:max-w-lg`，最大高度: `calc(100vh-2rem)`
- 支持垂直滚动 (`overflow-y-auto`)
- 动画: 淡入 + 缩放（zoom-in-95 / zoom-out-95）
- 右上角 X 关闭按钮（可通过 `showCloseButton` 控制显隐）

### `DialogHeader`

头部布局。Flex 纵向排列，居中对齐（小屏）/ 左对齐（大屏）。

### `DialogFooter`

底部布局。核心属性：

- **额外 Props**: `showCloseButton?: boolean`（默认 `false`）
- Flex 布局，小屏反向纵排，大屏横排右对齐
- 可选内置关闭按钮（使用 outline 变体 Button）

### `DialogTitle`

标题。大号字体 (`text-lg`)，半粗体。

### `DialogDescription`

描述。柔和前景色，小号字体。

## 注意事项

- 通过 Radix 原语提供完整的无障碍支持（焦点陷阱、Escape 关闭、屏幕阅读器）
- `DialogContent` 自动包裹 `DialogPortal` 和 `DialogOverlay`
- 被 10 个模态组件引用，是对话框场景的统一基础设施
- `DialogFooter` 的 `showCloseButton` 提供了额外的关闭方式，使用了 Radix `Close` + `asChild` + `Button` 的组合模式
