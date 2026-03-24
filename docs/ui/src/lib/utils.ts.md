# `utils.ts` -- 通用工具函数

> 源文件路径: `ui/src/lib/utils.ts`

## 功能概述

提供 CSS 类名合并工具函数 `cn`，结合 `clsx` 的条件类名和 `tailwind-merge` 的 Tailwind 类冲突解析能力。这是整个 UI 组件库的基础工具函数，几乎所有 UI 基础组件都依赖它。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `clsx` | 条件类名拼接库，支持对象、数组、字符串等多种格式 |
| `tailwind-merge` | Tailwind CSS 类名合并库，智能处理冲突（如 `p-2` 与 `p-4` 只保留后者） |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `components/ui/alert.tsx` | `cn` |
| `components/ui/badge.tsx` | `cn` |
| `components/ui/button.tsx` | `cn` |
| `components/ui/card.tsx` | `cn` |
| `components/ui/checkbox.tsx` | `cn` |
| `components/ui/dialog.tsx` | `cn` |
| `components/ui/dropdown-menu.tsx` | `cn` |
| `components/ui/input.tsx` | `cn` |
| `components/ui/label.tsx` | `cn` |
| `components/ui/separator.tsx` | `cn` |
| `components/ui/slider.tsx` | `cn` |
| `components/ui/switch.tsx` | `cn` |
| `components/ui/textarea.tsx` | `cn` |
| `components/ui/tooltip.tsx` | `cn` |

## 导出函数

### `cn(...inputs: ClassValue[]): string`

合并并去重 CSS 类名，智能解决 Tailwind CSS 类冲突。

**参数:**
- `inputs` -- 任意数量的 `ClassValue`（字符串、对象、数组、undefined 等）

**返回值:** 合并后的类名字符串

**示例:**

```typescript
cn("p-2 bg-red-500", "p-4")
// => "bg-red-500 p-4"  (p-2 被 p-4 覆盖)

cn("border", condition && "border-red-500")
// => "border border-red-500" 或 "border"

cn("text-sm", undefined, false, "font-bold")
// => "text-sm font-bold"
```

## 注意事项

- 这是 shadcn/ui 组件系统的标准模式，几乎所有组件都通过 `cn` 合并 `className` prop 与默认样式
- `twMerge` 确保后传入的 Tailwind 类能正确覆盖先前的同类属性，避免样式冲突
- `clsx` 处理条件类名的简洁语法（falsy 值自动过滤）
