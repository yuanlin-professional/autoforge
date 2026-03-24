# `alert.tsx` -- 警告提示组件

> 源文件路径: `ui/src/components/ui/alert.tsx`

## 功能概述

基于原生 HTML `div` 元素封装的警告提示组件，使用 `class-variance-authority` (CVA) 管理样式变体。支持默认和破坏性两种视觉变体，提供网格布局以自动适配 SVG 图标。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `class-variance-authority` | 样式变体管理（CVA） |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `SettingsModal.tsx` | `Alert`, `AlertDescription` |
| `SetupWizard.tsx` | `Alert`, `AlertDescription`, `AlertTitle` |
| `ExpandProjectChat.tsx` | `Alert`, `AlertDescription` |
| `HumanInputForm.tsx` | `Alert`, `AlertDescription` |
| `EditFeatureForm.tsx` | `Alert`, `AlertDescription` |
| `SpecCreationChat.tsx` | `Alert`, `AlertDescription` |
| `ResetProjectModal.tsx` | `Alert`, `AlertDescription` |
| `AddFeatureForm.tsx` | `Alert`, `AlertDescription` |
| `FeatureModal.tsx` | `Alert`, `AlertDescription` |
| `NewProjectModal.tsx` | `Alert`, `AlertDescription` |
| `ScheduleModal.tsx` | `Alert`, `AlertDescription` |

## 组件 Props / Variants

### `Alert`

- **Props**: `React.ComponentProps<"div">` + `VariantProps<typeof alertVariants>`
- **Variants**:
  - `default` -- 卡片背景色与文字色 (`bg-card text-card-foreground`)
  - `destructive` -- 破坏性红色文字 (`text-destructive bg-card`)
- **布局**: CSS Grid，包含 SVG 图标时自动显示图标列（`has-[>svg]:grid-cols-[calc(var(--spacing)*4)_1fr]`）
- **无障碍**: 自动添加 `role="alert"` 属性

### `AlertTitle`

- **Props**: `React.ComponentProps<"div">`
- 显示在第二列，单行截断，中等字重

### `AlertDescription`

- **Props**: `React.ComponentProps<"div">`
- 柔和前景色文字，显示在第二列

## 注意事项

- 非 Radix 原语组件，纯 HTML 元素封装
- 通过 `data-slot` 属性标记组件角色，便于外部样式定位
- 是项目中使用最广泛的反馈组件之一，被 11 个组件引用
