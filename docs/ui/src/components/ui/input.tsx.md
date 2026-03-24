# `input.tsx` -- 输入框组件

> 源文件路径: `ui/src/components/ui/input.tsx`

## 功能概述

基于原生 HTML `input` 元素封装的单行输入框组件，提供统一的样式、聚焦状态、文件上传样式、无障碍错误状态和暗色模式支持。无 Radix 原语依赖。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `DevServerConfigDialog.tsx` | `Input` |
| `ScheduleModal.tsx` | `Input` |
| `EditFeatureForm.tsx` | `Input` |
| `TerminalTabs.tsx` | `Input` |
| `QuestionOptions.tsx` | `Input` |
| `HumanInputForm.tsx` | `Input` |
| `AddFeatureForm.tsx` | `Input` |
| `NewProjectModal.tsx` | `Input` |
| `ExpandProjectChat.tsx` | `Input` |
| `FolderBrowser.tsx` | `Input` |

## 组件 Props / Variants

### `Input`

- **Props**: `React.ComponentProps<"input">`（继承原生 input 全部属性）
- **无变体**，单一样式
- **样式特性**:
  - 高度: `h-9`，圆角 `rounded-md`
  - 边框: `border-input`，暗色模式下半透明背景 (`dark:bg-input/30`)
  - 聚焦: `focus-visible:border-ring focus-visible:ring-[3px]`
  - 文件上传: `file:` 前缀样式（行内弹性布局、透明背景、中等字重）
  - 占位符: `placeholder:text-muted-foreground`
  - 选中文字: `selection:bg-primary selection:text-primary-foreground`
  - 禁用: 降低透明度 + 禁止点击
  - 响应式字号: 基础 `text-base`，md 断点 `text-sm`
  - 无障碍: `aria-invalid` 时显示红色边框和环

## 注意事项

- 被 10 个表单组件引用，是表单场景的基础输入控件
- 完全透传原生 `input` 属性，包括 `type` 参数
- 支持 `min-w-0` 防止在 flex 布局中溢出
- 使用 `shadow-xs` 提供微妙的深度感
