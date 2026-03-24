# `textarea.tsx` -- 多行文本输入框组件

> 源文件路径: `ui/src/components/ui/textarea.tsx`

## 功能概述

基于原生 HTML `textarea` 元素封装的多行文本输入组件，提供与 `Input` 组件一致的视觉风格。支持自动内容尺寸适配 (`field-sizing-content`)，无 Radix 原语依赖。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `EditFeatureForm.tsx` | `Textarea` |
| `AssistantChat.tsx` | `Textarea` |
| `AddFeatureForm.tsx` | `Textarea` |
| `SpecCreationChat.tsx` | `Textarea` |
| `HumanInputForm.tsx` | `Textarea` |

## 组件 Props / Variants

### `Textarea`

- **Props**: `React.ComponentProps<"textarea">`（继承原生 textarea 全部属性）
- **无变体**，单一样式
- **样式特性**:
  - 最小高度: `min-h-16` (64px)
  - 圆角: `rounded-md`
  - 边框: `border-input`
  - 内边距: `px-3 py-2`
  - 暗色模式: `dark:bg-input/30`（半透明背景）
  - 聚焦: `focus-visible:border-ring focus-visible:ring-[3px]`
  - 内容自适应: `field-sizing-content`（CSS 属性，textarea 高度随内容自动增长）
  - 禁用: `disabled:cursor-not-allowed disabled:opacity-50`
  - 响应式字号: 基础 `text-base`，md 断点 `text-sm`
  - 无障碍: `aria-invalid` 时显示红色边框和环
  - 阴影: `shadow-xs`

## 注意事项

- 被 5 个组件引用，主要用于聊天输入和表单的长文本编辑场景
- 使用 `field-sizing-content` CSS 属性实现高度自适应，这是较新的 CSS 特性
- 使用 `flex` 布局确保在各种容器中正确渲染
- 样式与 `Input` 组件保持高度一致，遵循统一的表单设计语言
