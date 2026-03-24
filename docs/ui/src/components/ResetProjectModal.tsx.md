# `ResetProjectModal.tsx` — 项目重置模态对话框

> 源文件路径: `ui/src/components/ResetProjectModal.tsx`

## 功能概述

`ResetProjectModal` 提供项目重置功能的确认界面，支持两种重置模式：**快速重置**（保留 App Spec 和提示词）和**完全重置**（删除所有配置）。组件清晰地列出每种模式下将被删除和保留的内容，帮助用户做出明确的选择。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `react` | `useState` |
| `lucide-react` | `Loader2`, `AlertTriangle`, `RotateCcw`, `Trash2`, `Check`, `X` 图标 |
| `../hooks/useProjects` | `useResetProject` — 项目重置的 mutation hook |
| `@/components/ui/dialog` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogFooter` |
| `@/components/ui/button` | `Button` |
| `@/components/ui/alert` | `Alert`, `AlertDescription` |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `App.tsx` | 在主应用中作为项目重置的确认对话框 |

## 关键组件/函数

### `ResetProjectModal`

- **Props**: `isOpen`、`projectName`、`onClose`、`onResetComplete`（重置完成回调，参数为是否全量重置）
- **状态管理**:
  - `resetType` — 当前选择的重置类型（`'quick'` 或 `'full'`）
- **交互逻辑**:
  - 顶部分段按钮切换快速/完全重置模式
  - 警告区域根据选择动态显示删除和保留的内容清单
  - 完全重置按钮使用 `destructive` 样式，快速重置使用默认样式
  - 重置进行中禁用所有按钮，防止重复操作
  - 关闭时重置 mutation 状态和 `resetType`

## 架构图

```mermaid
graph TD
    App[App.tsx] -->|isOpen, projectName| RPM[ResetProjectModal]
    RPM -->|mutateAsync| URP[useResetProject hook]
    URP -->|POST| API[/api/projects/reset]
    RPM -->|onResetComplete| App
```

## 注意事项

- 快速重置删除：功能特性、聊天历史、Agent 设置；保留：App Spec、项目代码
- 完全重置额外删除：App Spec 和提示词；重置后会触发 Setup Wizard
- 关闭对话框时调用 `resetProject.reset()` 清除可能的错误状态
