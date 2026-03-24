# `label.tsx` -- 标签组件

> 源文件路径: `ui/src/components/ui/label.tsx`

## 功能概述

基于 `@radix-ui/react-label` 原语封装的表单标签组件，提供正确的无障碍关联（通过 `htmlFor` 与输入控件关联）以及与 `peer` / `group` 联动的禁用状态样式。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@radix-ui/react-label` | Radix UI 标签原语 |
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `SettingsModal.tsx` | `Label` |
| `HumanInputForm.tsx` | `Label` |
| `ScheduleModal.tsx` | `Label` |
| `NewProjectModal.tsx` | `Label` |
| `AddFeatureForm.tsx` | `Label` |
| `DevServerConfigDialog.tsx` | `Label` |
| `EditFeatureForm.tsx` | `Label` |

## 组件 Props / Variants

### `Label`

- **Props**: `React.ComponentProps<typeof LabelPrimitive.Root>`（继承 Radix Label 全部 Props）
- **无变体**，单一样式
- **样式特性**:
  - 弹性布局 (`flex items-center gap-2`)
  - 小号字体 (`text-sm`)、中等字重 (`font-medium`)
  - 行高: `leading-none`
  - 文本不可选择: `select-none`
  - 与表单组件联动:
    - `group-data-[disabled=true]:pointer-events-none` -- 父级 group 禁用时跟随
    - `group-data-[disabled=true]:opacity-50` -- 父级 group 禁用时降低透明度
    - `peer-disabled:cursor-not-allowed` -- 相邻 peer 禁用时显示禁止光标
    - `peer-disabled:opacity-50` -- 相邻 peer 禁用时降低透明度

## 注意事项

- 通过 Radix 原语确保点击标签时正确聚焦关联的输入控件
- 被 7 个表单组件引用，通常与 `Input`、`Textarea`、`Switch`、`Slider` 等配合使用
- 支持通过 Tailwind 的 `peer` 和 `group` 机制与相邻/父级元素联动
