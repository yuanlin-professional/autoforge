# `keyboard.ts` -- 键盘事件工具函数

> 源文件路径: `ui/src/lib/keyboard.ts`

## 功能概述

提供键盘事件处理的工具函数，核心功能是 IME（输入法）感知的 Enter 键判断。在中日韩等语言输入时，用户需要通过 Enter 键确认候选字符，此模块确保在 IME 组合状态下不会意外触发表单提交。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| React (类型) | 使用 `React.KeyboardEvent` 类型 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `components/ExpandProjectChat.tsx` | `isSubmitEnter` |
| `components/FolderBrowser.tsx` | `isSubmitEnter` |
| `components/AssistantChat.tsx` | `isSubmitEnter` |
| `components/SpecCreationChat.tsx` | `isSubmitEnter` |
| `components/TerminalTabs.tsx` | `isSubmitEnter` |

## 导出函数

### `isSubmitEnter(e, allowShiftEnter?): boolean`

判断 Enter 按键事件是否应触发提交操作。

**参数:**

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `e` | `React.KeyboardEvent` | - | React 键盘事件对象 |
| `allowShiftEnter` | `boolean` | `true` | 若为 `true`，Shift+Enter 返回 `false`（用于多行输入换行） |

**判断逻辑:**

1. 按键不是 `Enter` -- 返回 `false`
2. `allowShiftEnter` 为 `true` 且按住 Shift -- 返回 `false`（允许换行）
3. `e.nativeEvent.isComposing` 为 `true`（IME 组合中） -- 返回 `false`
4. 以上条件均不满足 -- 返回 `true`（应当提交）

**使用场景:**

- **聊天输入框**: `isSubmitEnter(e)` -- Enter 发送，Shift+Enter 换行
- **单行搜索框**: `isSubmitEnter(e, false)` -- Enter 始终提交

## 注意事项

- 依赖 `nativeEvent.isComposing` 属性检测 IME 状态，这是原生 DOM 事件的属性，React 合成事件通过 `nativeEvent` 访问
- 在中文、日文、韩文输入法激活时，用户按 Enter 选择候选字符不会触发提交
- 所有聊天类组件和文件夹浏览器的搜索输入均使用此函数
