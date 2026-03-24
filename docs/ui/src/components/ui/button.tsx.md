# `button.tsx` -- 按钮组件

> 源文件路径: `ui/src/components/ui/button.tsx`

## 功能概述

基于 `button` 元素封装的多变体按钮组件，使用 CVA 管理 6 种视觉变体和 8 种尺寸变体。支持通过 `asChild` 模式（Radix Slot）将按钮样式委托给子元素（如 `<a>` 标签），是整个 UI 系统中使用最广泛的组件。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-slot` | `Slot` 组件，用于 `asChild` 模式 |
| `class-variance-authority` | 样式变体管理 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

被 34 个组件引用，以下列出主要使用者：

| 模块 | 引用内容 |
|------|----------|
| `App.tsx` | `Button` |
| `components/ui/dialog.tsx` | `Button`（DialogFooter 关闭按钮） |
| `AgentControl.tsx` | `Button` |
| `AgentMissionControl.tsx` | `Button` |
| `SettingsModal.tsx` | `Button` |
| `ScheduleModal.tsx` | `Button` |
| `NewProjectModal.tsx` | `Button` |
| `FeatureModal.tsx` | `Button` |
| `AddFeatureForm.tsx` | `Button` |
| `EditFeatureForm.tsx` | `Button` |
| `ConfirmDialog.tsx` | `Button` |
| `DependencyGraph.tsx` | `Button` |
| `ProjectSelector.tsx` | `Button` |
| `TerminalTabs.tsx` | `Button` |
| `AssistantChat.tsx` | `Button` |
| `SpecCreationChat.tsx` | `Button` |
| `ExpandProjectChat.tsx` | `Button` |
| `FolderBrowser.tsx` | `Button` |
| 其他 16 个组件... | `Button` |

## 组件 Props / Variants

### `Button`

- **Props**: `React.ComponentProps<"button">` + `VariantProps<typeof buttonVariants>` + `{ asChild?: boolean }`

**视觉变体 (`variant`):**

| 变体 | 说明 |
|------|------|
| `default` | 主色实心背景 (`bg-primary text-primary-foreground`) |
| `destructive` | 破坏性红色 (`bg-destructive text-white`) |
| `outline` | 边框样式，悬停强调色背景 |
| `secondary` | 次要色背景 |
| `ghost` | 透明背景，悬停时显示强调色 |
| `link` | 链接样式，带下划线 |

**尺寸变体 (`size`):**

| 尺寸 | 说明 |
|------|------|
| `default` | 标准尺寸 (h-9, px-4) |
| `xs` | 超小 (h-6, px-2, text-xs) |
| `sm` | 小号 (h-8, px-3) |
| `lg` | 大号 (h-10, px-6) |
| `icon` | 正方形图标按钮 (size-9) |
| `icon-xs` | 超小图标 (size-6) |
| `icon-sm` | 小图标 (size-8) |
| `icon-lg` | 大图标 (size-10) |

### 导出

同时导出 `Button` 组件和 `buttonVariants` 函数。

## 注意事项

- 是整个项目中被引用次数最多的组件（34 个文件），是 UI 系统的核心构建块
- 包含 SVG 图标按钮自动调整内边距的特殊选择器（`has-[>svg]:px-*`）
- 禁用状态统一处理：`disabled:pointer-events-none disabled:opacity-50`
- 通过 `data-slot="button"`、`data-variant`、`data-size` 暴露组件元数据
