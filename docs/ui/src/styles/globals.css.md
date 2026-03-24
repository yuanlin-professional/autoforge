# `globals.css` -- 全局样式与主题系统

> 源文件路径: `ui/src/styles/globals.css`

## 功能概述

定义应用的全局样式表，包含多主题色彩系统、Tailwind CSS v4 主题集成、自定义动画、文档排版样式、聊天消息排版样式、滚动条定制以及实用工具类。通过 CSS 自定义属性（变量）实现主题切换，支持明暗模式。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `tailwindcss` | Tailwind CSS v4 框架 |
| `tw-animate-css` | Tailwind 动画插件 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `main.tsx` | 通过 `import './styles/globals.css'` 引入全局样式 |

## 主题系统

### 可用主题

| 主题 | CSS 类名 | 风格描述 | 圆角半径 |
|------|----------|----------|----------|
| Twitter（默认） | `:root` | 清爽现代蓝色设计 | 0.625rem |
| Claude | `.theme-claude` | 暖色米色/奶油色调，橙色主色 | 0.5rem |
| Neo Brutalism | `.theme-neo-brutalism` | 粗犷风格，硬阴影，无圆角 | 0px |
| Retro Arcade | `.theme-retro-arcade` | 活力粉色与青色，像素风灵感 | 0.25rem |
| Aurora | `.theme-aurora` | 深紫与荧光青，北极光灵感 | 0.5rem |
| Business | `.theme-business` | 专业深藏青(#000e4e)与灰色调 | 0.5rem |

每个主题均提供完整的 **亮色** 和 **暗色** 模式变量（通过 `.dark` 类切换）。

### CSS 变量体系

每个主题定义以下分类的 CSS 变量：

**色彩变量（oklch 色彩空间）：**
- `--background` / `--foreground` -- 页面背景/前景色
- `--card` / `--card-foreground` -- 卡片背景/前景色
- `--popover` / `--popover-foreground` -- 弹出层背景/前景色
- `--primary` / `--primary-foreground` -- 主色/主色前景
- `--secondary` / `--secondary-foreground` -- 次要色
- `--muted` / `--muted-foreground` -- 柔和色
- `--accent` / `--accent-foreground` -- 强调色
- `--destructive` / `--destructive-foreground` -- 危险/破坏色
- `--border` -- 边框色
- `--input` -- 输入框背景色
- `--ring` -- 聚焦环色
- `--chart-1` ~ `--chart-5` -- 图表色系列
- `--sidebar-*` -- 侧边栏专用色系

**阴影变量：**
- `--shadow-sm` / `--shadow` / `--shadow-md` / `--shadow-lg` -- 四级阴影（各主题风格不同，Neo Brutalism 为硬阴影）

**状态色变量：**
- `--color-status-pending` -- 待处理状态色（用于看板）
- `--color-status-progress` -- 进行中状态色
- `--color-status-done` -- 完成状态色

**日志级别色变量：**
- `--color-log-error` / `--color-log-warning` / `--color-log-info` / `--color-log-debug` / `--color-log-success`

**字体变量：**
- `--font-sans` -- 各主题使用不同无衬线字体（Open Sans / DM Sans / Outfit / Inter / 系统字体）
- `--font-mono` -- 等宽字体栈

**过渡变量：**
- `--transition-fast` -- 150ms
- `--transition-normal` -- 250ms
- `--ease-smooth` -- cubic-bezier(0.4, 0, 0.2, 1)

## ShadCN Tailwind v4 集成

通过 `@theme inline` 指令将 CSS 变量映射到 Tailwind v4 的主题系统，使得可以在 Tailwind 类名中直接使用主题变量（如 `bg-primary`、`text-muted-foreground`、`shadow-md` 等）。同时定义了响应式圆角变量 `--radius-sm` 到 `--radius-4xl`。

## 动画系统

### 基础动画

| 关键帧名称 | CSS 工具类 | 说明 |
|-----------|-----------|------|
| `popIn` | `.animate-pop-in` | 缩放弹入（0.95 -> 1） |
| `slideIn` | `.animate-slide-in` | 从左滑入 |
| `slideInUp` | `.animate-slide-in-up` | 从下滑入 |
| `slideInDown` | `.animate-slide-in-down` | 从上滑入 |
| `fadeIn` | `.animate-fade-in` | 淡入 |
| `shimmer` | `.animate-shimmer` | 文字闪光效果（渐变背景裁剪） |
| `spin` | `.animate-spin` | 旋转 |
| `pulse` | `.animate-pulse` | 脉冲（透明度变化） |
| `bounce` | `.animate-bounce` | 弹跳 |

### Agent 吉祥物动画

| 关键帧名称 | CSS 工具类 | 说明 |
|-----------|-----------|------|
| `thinking` | `.animate-thinking` | 思考状态（轻微上下浮动+缩放） |
| `working` | `.animate-working` | 工作状态（左右微振） |
| `testing` | `.animate-testing` | 测试状态（轻微左右摇摆旋转） |
| `celebrate` | `.animate-celebrate` | 庆祝状态（放大+旋转） |
| `shake` | `.animate-shake` | 错误抖动 |
| `confetti` | `.animate-confetti` | 彩纸掉落 |

### 编排器（Maestro）动画

| 关键帧名称 | CSS 工具类 | 说明 |
|-----------|-----------|------|
| `conducting` | `.animate-conducting` | 指挥动作 |
| `batonTap` | `.animate-baton-tap` | 指挥棒点击 |
| - | `.animate-maestro-idle` | 空闲状态（复用 bounce） |
| - | `.animate-maestro-complete` | 完成状态（复用 celebrate） |

### 交错延迟

`.stagger-1` 到 `.stagger-5` 提供 50ms 递增的动画延迟（50ms ~ 250ms），用于列表项顺序动画。

## 排版系统

### `.docs-prose` -- 文档页面排版

用于 `/#/docs` 路由的文档内容渲染，设定标题、段落、列表、代码块、表格、引用块等完整的排版规则。段落最大宽度 65ch，行高 1.7。

### `.chat-prose` -- 聊天消息排版

用于聊天气泡中 Markdown 内容渲染，整体更紧凑（行高 1.6，更小的间距）。`.chat-prose-user` 子类为用户消息气泡提供白色半透明样式覆盖，以在主色背景上保持对比度。

## 其他全局样式

- **基础层**：所有元素应用 `border-border`，body 应用背景色、前景色和字体平滑设置
- **主题过渡**：`:root` 上添加 0.2s 的背景色和文字颜色过渡，实现平滑主题切换
- **滚动条**：自定义 WebKit 滚动条样式（8px 宽，使用主题边框色）
- **滑块**：`.slider-input` 类定制原生 range input 的滑块样式（圆形滑块、圆角轨道）
- **Neo Brutalism 工具类**：`.shadow-neo` / `.shadow-neo-sm` / `.shadow-neo-lg`

## 注意事项

- 使用 oklch 色彩空间定义颜色，提供更均匀的色彩感知
- 暗色模式通过 `@custom-variant dark (&:where(.dark, .dark *))` 启用基于类名的切换
- 所有主题的字体栈各不相同，切换主题时字体也会随之变化
- Neo Brutalism 主题的阴影为硬实阴影（无模糊），暗色模式下使用白色半透明阴影
- Business 主题使用更明显的阴影以营造卡片深度感
