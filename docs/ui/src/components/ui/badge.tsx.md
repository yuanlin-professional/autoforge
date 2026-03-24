# `badge.tsx` -- 徽章组件

> 源文件路径: `ui/src/components/ui/badge.tsx`

## 功能概述

基于 `span` 元素封装的行内徽章组件，使用 CVA 管理 6 种样式变体。支持通过 `asChild` 模式（Radix Slot）将样式委托给子元素，常用于状态标签、标记和分类显示。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-slot` | `Slot` 组件，用于 `asChild` 模式 |
| `class-variance-authority` | 样式变体管理 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `FeatureCard.tsx` | `Badge` |
| `ProgressDashboard.tsx` | `Badge` |
| `QuestionOptions.tsx` | `Badge` |
| `KanbanColumn.tsx` | `Badge` |
| `NewProjectModal.tsx` | `Badge` |
| `FeatureModal.tsx` | `Badge` |
| `ProjectSelector.tsx` | `Badge` |
| `OrchestratorStatusCard.tsx` | `Badge` |
| `DebugLogViewer.tsx` | `Badge` |
| `KeyboardShortcutsHelp.tsx` | `Badge` |
| `DependencyBadge.tsx` | `Badge` |
| `App.tsx` | `Badge` |
| `AgentCard.tsx` | `Badge` |
| `AgentMissionControl.tsx` | `Badge` |
| `AgentControl.tsx` | `Badge` |

## 组件 Props / Variants

### `Badge`

- **Props**: `React.ComponentProps<"span">` + `VariantProps<typeof badgeVariants>` + `{ asChild?: boolean }`
- **Variants**:
  - `default` -- 主色背景 (`bg-primary text-primary-foreground`)
  - `secondary` -- 次要色背景 (`bg-secondary text-secondary-foreground`)
  - `destructive` -- 破坏性红色 (`bg-destructive text-white`)
  - `outline` -- 边框样式 (`border-border text-foreground`)
  - `ghost` -- 透明背景，悬停时显示强调色
  - `link` -- 链接样式，带下划线偏移
- **样式特性**: 圆角全圆 (`rounded-full`)、内联弹性布局、文本不换行、SVG 图标自适应
- **`asChild`**: 为 `true` 时使用 Radix `Slot`，将 Badge 的样式应用到子元素上

### 导出

同时导出 `Badge` 组件和 `badgeVariants` 函数（可在外部使用变体样式）。

## 注意事项

- 是项目中引用最广泛的 UI 组件之一（15 个文件引用）
- 通过 `data-variant` 属性暴露当前变体，便于外部选择器定位
- 支持焦点可见环和 `aria-invalid` 无障碍错误状态
