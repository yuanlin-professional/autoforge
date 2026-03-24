# `card.tsx` -- 卡片组件

> 源文件路径: `ui/src/components/ui/card.tsx`

## 功能概述

基于原生 HTML `div` 元素封装的卡片组件系列，提供完整的卡片布局结构（容器、头部、标题、描述、操作区、内容区、底部）。无 Radix 原语依赖，纯 CSS 样式封装。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

被 22 个组件引用，是 UI 系统中使用第二广泛的组件：

| 模块 | 引用内容 |
|------|----------|
| `App.tsx` | `Card`, `CardContent` |
| `ActivityFeed.tsx` | `Card`, `CardContent` |
| `FeatureCard.tsx` | `Card`, `CardContent` |
| `ProgressDashboard.tsx` | `Card`, `CardContent`, `CardHeader`, `CardTitle` |
| `KanbanColumn.tsx` | `Card`, `CardContent`, `CardHeader`, `CardTitle` |
| `KanbanBoard.tsx` | `Card`, `CardContent` |
| `DependencyGraph.tsx` | `Card`, `CardContent` |
| `AgentCard.tsx` | `Card`, `CardContent` |
| `AgentMissionControl.tsx` | `Card`, `CardContent` |
| `OrchestratorStatusCard.tsx` | `Card`, `CardContent` |
| `ProjectSetupRequired.tsx` | `Card`, `CardContent`, `CardDescription`, `CardHeader`, `CardTitle` |
| `ConversationHistory.tsx` | `Card`, `CardContent`, `CardHeader` |
| `NewProjectModal.tsx` | `Card`, `CardContent` |
| `ScheduleModal.tsx` | `Card`, `CardContent` |
| `FolderBrowser.tsx` | `Card`, `CardContent` |
| 其他 7 个组件... | `Card` 及子组件 |

## 组件 Props / Variants

所有子组件均接受 `React.ComponentProps<"div">`，无额外变体。

### `Card`

卡片容器。样式：卡片背景色、flex 纵向布局、6 间距、圆角 xl、边框、小号阴影。

### `CardHeader`

卡片头部。CSS Grid 自适应布局，当包含 `CardAction` 时自动切换为两列布局。使用 `@container/card-header` 容器查询。

### `CardTitle`

卡片标题。`font-semibold`，`leading-none`。

### `CardDescription`

卡片描述。柔和前景色，`text-sm`。

### `CardAction`

卡片操作区。定位于头部网格的右上角（第二列，跨两行）。

### `CardContent`

卡片内容区。水平内边距 `px-6`。

### `CardFooter`

卡片底部。`flex` 水平布局，水平内边距 `px-6`。

## 注意事项

- 所有子组件通过 `data-slot` 属性标记，支持外部选择器精确定位
- `CardHeader` 支持 `has-data-[slot=card-action]` 选择器自动响应操作按钮的存在
- `CardFooter` 和 `CardHeader` 支持 `.border-b` / `.border-t` 条件内边距
- 是纯展示型组件，无任何状态管理或交互逻辑
