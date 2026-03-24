# `timeUtils.ts` -- 时间与时区转换工具

> 源文件路径: `ui/src/lib/timeUtils.ts`

## 功能概述

提供 UTC 与本地时间之间的双向转换工具，以及星期位域（bitfield）操作函数。主要用于调度管理功能中，数据库存储 UTC 时间，界面显示用户本地时区时间。当时间转换跨越日期边界时（如东京 UTC+9 的 02:00 本地时间对应 UTC 前一天 17:00），自动调整星期位域。

## 依赖关系

### 导入依赖

无外部依赖，纯工具函数模块。

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `components/ScheduleModal.tsx` | `utcToLocal`, `localToUTCWithDayShift`, `adjustDaysForDayShift`, `DAYS`, `isDayActive`, `toggleDay`, `formatDaysDescription`, `formatDuration` |
| `components/AgentControl.tsx` | `formatNextRun`, `formatEndTime` |

## 类型定义

### `TimeConversionResult`

```typescript
interface TimeConversionResult {
  time: string        // "HH:MM" 格式的时间字符串
  dayShift: -1 | 0 | 1  // -1=前一天, 0=同一天, 1=后一天
}
```

## 导出函数

### 时间转换

| 函数 | 说明 |
|------|------|
| `utcToLocalWithDayShift(utcTime)` | UTC 时间转本地时间，返回 `TimeConversionResult`（含日期偏移信息） |
| `utcToLocal(utcTime)` | UTC 转本地时间的简化版本（向后兼容），仅返回时间字符串 |
| `localToUTCWithDayShift(localTime)` | 本地时间转 UTC，返回 `TimeConversionResult`（含日期偏移信息） |
| `localToUTC(localTime)` | 本地转 UTC 的简化版本（向后兼容），仅返回时间字符串 |

### 星期位域操作

采用 7 位位域表示星期：Mon=1, Tue=2, Wed=4, Thu=8, Fri=16, Sat=32, Sun=64。

| 函数 | 说明 |
|------|------|
| `shiftDaysForward(bitfield)` | 位域整体前移一天（Mon->Tue, Sun->Mon） |
| `shiftDaysBackward(bitfield)` | 位域整体后移一天（Tue->Mon, Mon->Sun） |
| `adjustDaysForDayShift(bitfield, dayShift)` | 根据日期偏移自动调整位域 |
| `isDayActive(bitfield, dayBit)` | 检查某天是否在位域中激活 |
| `toggleDay(bitfield, dayBit)` | 切换位域中某天的状态 |

### 格式化函数

| 函数 | 说明 | 示例输出 |
|------|------|----------|
| `formatDuration(minutes)` | 将分钟数格式化为可读字符串 | `"4h"`, `"1h 30m"`, `"30m"` |
| `formatNextRun(isoString)` | 格式化下次运行时间（24小时内仅显示时间，否则含星期） | `"22:00"`, `"Mon 22:00"` |
| `formatEndTime(isoString)` | 格式化结束时间 | `"14:00"`, `"2:00 PM"` |
| `formatDaysDescription(bitfield)` | 将位域转为可读描述 | `"Every day"`, `"Weekdays"`, `"Mon, Wed, Fri"` |

## 导出常量

| 常量 | 说明 |
|------|------|
| `DAY_BITS` | 星期位值映射对象 `{ Mon: 1, Tue: 2, ... Sun: 64 }` |
| `DAYS` | 星期数组 `[{ label: 'Mon', bit: 1 }, ...]`，用于 UI 遍历渲染 |

## 注意事项

- 时间转换使用固定参考日期（2000-01-15）来计算日期偏移，避免夏令时等复杂情况
- `formatNextRun` 和 `formatEndTime` 使用浏览器的 `toLocaleTimeString`，会根据用户系统设置自动切换 12/24 小时制
- 位域操作采用位运算（`&`, `|`, `^`），性能高效
- 特殊位域值：127 = 每天, 31 = 工作日, 96 = 周末
