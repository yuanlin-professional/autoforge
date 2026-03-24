# `SpecCreationChat.tsx` -- 交互式项目规格创建聊天界面

> 源文件路径: `ui/src/components/SpecCreationChat.tsx`

## 功能概述

`SpecCreationChat` 是一个全屏聊天界面组件，用于通过与 Claude AI 的 7 阶段对话流程交互式地创建应用规格（app_spec）。用户可以通过自然语言描述需求，Claude 会引导完成规格的各个方面，并最终生成完整的项目规格文件。

该组件支持富交互功能：图片附件（JPEG/PNG，最大 5MB）的上传、预览和拖拽、结构化问答表单（通过 `QuestionOptions` 组件）、示例提示词快速加载、WebSocket 实时连接状态显示、以及 YOLO 模式切换。聊天完成后，组件提供自动启动初始化 Agent 的功能，并包含失败重试机制。

用户还可以通过 `/exit` 命令或"Exit to Project"按钮随时退出聊天，直接进入项目页面手动启动 Agent。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `react` | `useCallback`, `useEffect`, `useRef`, `useState` -- React Hooks |
| `lucide-react` | 多个图标组件（Send, X, CheckCircle2, AlertCircle 等） |
| `../hooks/useSpecChat` | `useSpecChat` -- 规格创建聊天 WebSocket hook |
| `./ChatMessage` | 聊天消息渲染组件 |
| `./QuestionOptions` | 结构化问题选项组件 |
| `./TypingIndicator` | 打字指示器组件 |
| `../lib/types` | `ImageAttachment` 类型 |
| `../lib/keyboard` | `isSubmitEnter` -- 提交快捷键检测 |
| `@/components/ui/button` | Button 组件 |
| `@/components/ui/textarea` | Textarea 组件 |
| `@/components/ui/card` | Card, CardContent 组件 |
| `@/components/ui/alert` | Alert, AlertDescription 组件 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `ui/src/App.tsx` | 导入 `SpecCreationChat`，在主应用中直接使用 |
| `ui/src/components/NewProjectModal.tsx` | 导入 `SpecCreationChat`，在聊天步骤中渲染 |

## 关键组件/函数

### `SpecCreationChat`

**Props:**
- `projectName: string` -- 项目名称
- `onComplete: (specPath: string, yoloMode?: boolean) => void` -- 规格完成回调
- `onCancel: () => void` -- 取消回调
- `onExitToProject: () => void` -- 退出到项目页面回调
- `initializerStatus?: InitializerStatus` -- 初始化 Agent 状态
- `initializerError?: string | null` -- Agent 启动错误信息
- `onRetryInitializer?: () => void` -- 重试初始化回调

**状态管理:**
- `input` -- 用户输入文本
- `yoloEnabled` -- YOLO 模式开关
- `pendingAttachments: ImageAttachment[]` -- 待发送的图片附件
- 通过 `useSpecChat` hook 管理 WebSocket 连接和消息流

**核心交互:**
- `handleSendMessage()` -- 发送消息（支持纯文本和带附件），检测 `/exit` 命令
- `handleAnswerSubmit()` -- 提交结构化问答答案
- `handleFileSelect()` -- 图片选择验证（类型、大小限制）并转换为 Base64
- `handleDrop()` / `handleDragOver()` -- 拖拽上传支持
- `ConnectionIndicator` -- 内联连接状态指示器

**示例提示词:**
组件内置了一个 Simple Todo 应用的示例提示词 `SAMPLE_PROMPT`，包含约 25 个功能的完整描述，方便快速测试。

**完成区域:**
- 空闲状态：显示 YOLO 模式切换和"Continue to Project"按钮
- 启动中：显示旋转加载器
- 错误状态：显示错误信息和重试按钮
- 背景色根据状态变化（绿色/红色）

## 架构图

```mermaid
graph TD
    App[App.tsx] --> SCC[SpecCreationChat]
    NPM[NewProjectModal] --> SCC
    SCC --> USC[useSpecChat Hook]
    SCC --> CM[ChatMessage]
    SCC --> QO[QuestionOptions]
    SCC --> TI[TypingIndicator]

    USC --> WS[WebSocket 连接]

    subgraph "输入功能"
        Text[文本输入]
        Image[图片附件]
        Sample[示例提示词]
        Exit[/exit 命令]
    end

    subgraph "完成区域"
        YOLO[YOLO 模式切换]
        Continue[Continue 按钮]
        Retry[重试按钮]
    end
```

## 注意事项

- 图片附件限制：仅支持 JPEG 和 PNG 格式，单文件最大 5MB
- 图片通过 FileReader 转换为 Base64 DataURL，然后提取纯 Base64 数据发送
- Textarea 自动调整高度，最大 200px
- 输入框在 `isLoading && !currentQuestions` 时禁用，允许在等待问题回答时继续输入
- `/exit` 命令是客户端拦截处理，不会发送到 Claude
- 组件挂载时自动调用 `start()` 建立 WebSocket 连接，卸载时调用 `disconnect()`
