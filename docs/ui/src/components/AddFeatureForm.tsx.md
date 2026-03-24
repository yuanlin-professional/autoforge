# `AddFeatureForm.tsx` — 新增功能特性表单对话框

> 源文件路径: `ui/src/components/AddFeatureForm.tsx`

## 功能概述

`AddFeatureForm` 是一个模态对话框形式的表单组件，用于向项目中手动添加新的功能特性（Feature）。用户可以输入类别、名称、描述、优先级以及可选的测试步骤列表。表单通过 `useCreateFeature` hook 将数据提交到后端 API。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `react` | `useState`, `useId` 状态管理和唯一 ID 生成 |
| `lucide-react` | `X`, `Plus`, `Trash2`, `Loader2`, `AlertCircle` 图标 |
| `../hooks/useProjects` | `useCreateFeature` — 创建功能特性的 mutation hook |
| `@/components/ui/dialog` | `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogFooter` |
| `@/components/ui/button` | `Button` |
| `@/components/ui/input` | `Input` |
| `@/components/ui/textarea` | `Textarea` |
| `@/components/ui/label` | `Label` |
| `@/components/ui/alert` | `Alert`, `AlertDescription` |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `App.tsx` | 通过快捷键 `N` 或按钮触发，以对话框形式展示 |

## 关键组件/函数

### `AddFeatureForm`

- **Props**: `projectName`（目标项目名）、`onClose`（关闭回调）
- **状态管理**:
  - `category`, `name`, `description`, `priority` — 基本表单字段
  - `steps` — 动态测试步骤列表，每项有唯一 `id` 和 `value`
  - `error` — 提交失败时的错误信息
- **交互逻辑**:
  - 动态添加/删除测试步骤，使用 `useId` 生成稳定的 key
  - 表单验证：`category`、`name`、`description` 均为必填
  - 提交时过滤空步骤，优先级为可选（留空则自动分配）
  - 提交成功后自动关闭对话框

## 架构图

```mermaid
graph TD
    App[App.tsx] -->|projectName, onClose| AFF[AddFeatureForm]
    AFF -->|mutateAsync| UCF[useCreateFeature hook]
    UCF -->|POST| API[/api/features]
    AFF -->|UI 组件| Dialog[Dialog 组件]
```

## 注意事项

- 步骤列表使用 `useId` + 计数器组合生成唯一 key，确保 React 渲染稳定性
- 表单提交为异步操作，提交期间按钮显示加载动画并禁用
- 空白步骤在提交时自动过滤，不会发送到后端
