# `slider.tsx` -- 滑块组件

> 源文件路径: `ui/src/components/ui/slider.tsx`

## 功能概述

基于原生 HTML `<input type="range">` 封装的滑块组件，不使用 Radix 原语。提供受控值显示、自定义滑块样式（通过 `globals.css` 中的 `.slider-input` 类），以及与表单一致的聚焦和禁用状态。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `@/lib/utils` | `cn` 类名合并工具 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `SettingsModal.tsx` | `Slider` |

## 组件 Props / Variants

### `SliderProps` 接口

```typescript
interface SliderProps extends Omit<React.InputHTMLAttributes<HTMLInputElement>, 'onChange'> {
  min: number           // 最小值
  max: number           // 最大值
  value: number         // 当前值（受控）
  onChange: (value: number) => void  // 值变更回调（已转为 number 类型）
  label?: string        // 可选标签（当前未使用于渲染）
}
```

### `Slider`

- **Props**: `SliderProps`
- **无变体**，单一样式
- **渲染结构**:
  - 外层 `div`：`flex items-center gap-3`
  - `<input type="range">`：应用 `slider-input` 类
  - 值显示 `<span>`：定宽 `min-w-[2ch]`、居中、半粗体、等宽数字 (`tabular-nums`)
- **样式特性**:
  - 滑块轨道: 2px 高度、全圆角、`bg-input` 背景色
  - 滑块手柄: 通过 `globals.css` 中 `.slider-input::-webkit-slider-thumb` 和 `.slider-input::-moz-range-thumb` 定制
  - 聚焦: `focus-visible:ring-2 focus-visible:ring-ring`
  - 禁用: `cursor-not-allowed opacity-50`

## 注意事项

- `onChange` 的类型签名从 `React.ChangeEvent` 简化为 `(value: number) => void`，内部做了 `Number(e.target.value)` 转换
- 滑块的视觉样式（圆形手柄、主色背景、悬停放大等）定义在 `globals.css` 而非组件内
- 当前仅在 `SettingsModal.tsx` 中用于配置 batch size 等数值参数
- 值显示使用 `tabular-nums` 确保数字宽度一致，避免布局抖动
