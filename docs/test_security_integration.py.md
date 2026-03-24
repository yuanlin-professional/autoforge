# `test_security_integration.py` -- 安全策略集成测试

> 源文件路径: `test_security_integration.py`

## 功能概述

本文件是 AutoForge 安全模块的集成测试套件，与单元测试 (`test_security.py`) 不同，它聚焦于端到端场景验证：创建真实的临时项目结构、配置真实的 YAML 文件、通过安全钩子执行命令并验证行为。共包含 9 个集成测试用例。

测试覆盖了安全系统的完整层级：硬编码黑名单拦截、默认白名单放行、非白名单命令拦截、项目级配置允许自定义命令、通配符模式匹配、组织级黑名单不可覆盖性、组织级白名单继承、无效 YAML 容错降级、以及 100 条命令限制的强制执行。

每个测试都构建了完整的项目目录结构（包含 `.autoforge/` 目录和配置文件），模拟真实的运行环境，确保安全策略在实际部署场景中按预期工作。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `asyncio` | 运行异步安全钩子函数 |
| `os` | 操作环境变量（HOME 目录切换） |
| `sys` | 平台检测（Windows 兼容）和退出码 |
| `tempfile` | 创建临时项目目录和临时 HOME 目录 |
| `contextlib.contextmanager` | 实现 `temporary_home` 上下文管理器 |
| `pathlib.Path` | 文件路径操作 |
| `security.bash_security_hook` | 被测核心安全钩子函数 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `CLAUDE.md` | 项目文档中列为测试命令 (`python test_security_integration.py`) |
| `examples/README.md` | 示例文档中引用为集成测试参考 |

## 测试场景

### `test_blocked_command_via_hook()` (TEST 1)
- **目的**: 验证硬编码黑名单命令（sudo）被安全钩子正确拦截
- **场景**: 创建最小项目结构（空命令列表），尝试执行 `sudo apt install nginx`
- **关键断言**: 安全钩子返回 `decision: block`

### `test_allowed_command_via_hook()` (TEST 2)
- **目的**: 验证默认白名单中的命令（ls）被正确放行
- **场景**: 创建最小项目结构，尝试执行 `ls -la`
- **关键断言**: 安全钩子不返回 `block` 决策

### `test_non_allowed_command_via_hook()` (TEST 3)
- **目的**: 验证不在任何白名单中的命令（wget）被拦截
- **场景**: 创建最小项目结构（空命令列表），尝试执行 `wget https://example.com`
- **关键断言**: 安全钩子返回 `decision: block`

### `test_project_config_allows_command()` (TEST 4)
- **目的**: 验证项目配置中添加的自定义命令被正确放行
- **场景**: 创建包含 `swift` 和 `xcodebuild` 的项目配置，尝试执行 `swift --version`
- **关键断言**: 安全钩子不返回 `block` 决策

### `test_pattern_matching()` (TEST 5)
- **目的**: 验证通配符模式在安全钩子中的端到端工作
- **场景**: 创建包含 `swift*` 模式的项目配置，尝试执行 `swiftlint`
- **关键断言**: `swiftlint` 匹配 `swift*` 模式，安全钩子放行

### `test_org_blocklist_enforcement()` (TEST 6)
- **目的**: 验证组织级黑名单命令即使在项目配置中显式允许也无法执行
- **场景**: 组织配置阻止 `terraform`/`kubectl`，项目配置允许 `terraform`，尝试执行 `terraform apply`
- **关键断言**: 安全钩子返回 `block`（组织级黑名单优先级高于项目级白名单）

### `test_org_allowlist_inheritance()` (TEST 7)
- **目的**: 验证组织级白名单命令对所有项目可用
- **场景**: 组织配置允许 `jq`，项目配置为空命令列表，尝试执行 `jq '.data'`
- **关键断言**: `jq` 通过组织级白名单被放行

### `test_invalid_yaml_ignored()` (TEST 8)
- **目的**: 验证无效 YAML 配置被安全忽略，系统降级到默认白名单
- **场景**: 创建包含无效 YAML 内容的项目配置，尝试执行默认白名单中的 `ls`
- **关键断言**: 尽管配置无效，`ls` 仍然通过默认白名单被放行

### `test_100_command_limit()` (TEST 9)
- **目的**: 验证超过 100 条命令的配置被拒绝
- **场景**: 创建包含 101 条命令的项目配置，尝试执行其中的 `cmd0`
- **关键断言**: 配置被整体拒绝，`cmd0` 被安全钩子拦截

## 测试覆盖范围

- 硬编码黑名单端到端拦截
- 默认白名单端到端放行
- 非白名单命令端到端拦截
- 项目级自定义命令端到端放行
- 通配符模式端到端匹配
- 组织级黑名单不可覆盖性（安全层级优先级）
- 组织级白名单跨项目继承
- 无效配置容错降级
- 命令数量限制强制执行

## Fixtures 和辅助函数

| 名称 | 类型 | 说明 |
|------|------|------|
| `temporary_home(home_path)` | 上下文管理器 | 临时设置 HOME 环境变量（兼容 Unix/Windows），保存并恢复 HOME、USERPROFILE、HOMEDRIVE、HOMEPATH 四个环境变量 |

## 架构图

```mermaid
graph TD
    TI["test_security_integration.py<br/>集成测试（9 个）"]
    SEC["security.py<br/>安全模块"]
    YAML["allowed_commands.yaml<br/>项目级配置"]
    ORG["config.yaml<br/>组织级配置"]

    TI -->|调用| SEC
    TI -->|创建临时| YAML
    TI -->|创建临时| ORG

    subgraph 安全层级（由高到低）
        BL["硬编码黑名单<br/>sudo, shutdown, dd"]
        OB["组织级黑名单<br/>terraform, kubectl"]
        OA["组织级白名单<br/>jq"]
        GA["全局默认白名单<br/>ls, npm, git"]
        PA["项目级白名单<br/>swift, xcodebuild"]
    end

    SEC --> BL
    SEC --> OB
    SEC --> OA
    SEC --> GA
    SEC --> PA

    subgraph 测试场景
        T1["TEST 1: 黑名单拦截"]
        T2["TEST 2: 默认放行"]
        T3["TEST 3: 非白名单拦截"]
        T4["TEST 4: 项目命令放行"]
        T5["TEST 5: 通配符匹配"]
        T6["TEST 6: 组织黑名单不可覆盖"]
        T7["TEST 7: 组织白名单继承"]
        T8["TEST 8: 无效配置降级"]
        T9["TEST 9: 数量限制执行"]
    end

    T1 --> BL
    T2 --> GA
    T3 --> GA
    T4 --> PA
    T5 --> PA
    T6 --> OB
    T7 --> OA
    T8 --> GA
    T9 --> PA
```

## 注意事项

- 与 `test_security.py` 的区别：本文件侧重于端到端场景（创建真实目录和文件），而 `test_security.py` 侧重于函数级单元测试
- 每个测试都通过 `tempfile.TemporaryDirectory()` 创建完全隔离的项目结构，测试结束后自动清理
- TEST 6 是安全关键测试——验证组织级黑名单的不可覆盖性是纵深防御策略的核心
- TEST 8 验证了容错降级：即使配置损坏，系统仍能基于默认白名单安全运行
- `temporary_home` 上下文管理器与 `test_security.py` 中的实现完全相同，两个文件独立维护各自的副本
- 本测试未集成到 CI 流水线中（CI 仅运行 `test_security.py`），但在本地开发中是重要的验证手段
- 测试输出使用 emoji 标记（PASS/FAIL），便于快速识别结果
