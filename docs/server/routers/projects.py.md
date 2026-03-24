# `projects.py` -- 项目管理 API 路由

> 源文件路径: `server/routers/projects.py`

## 功能概述

`projects.py` 实现了项目管理的完整 CRUD API,是 AutoForge 服务器中最核心的路由之一。它提供项目的创建、查询、删除、重置以及提示词和设置的读写操作。所有端点都挂载在 `/api/projects` 前缀下。

该路由器使用项目注册表(registry)进行项目路径查找,而不是依赖固定的目录结构。每个操作都包含严格的名称验证、路径安全检查和状态校验(如检查代理是否正在运行)。项目的提示词文件(app_spec、initializer_prompt、coding_prompt)可通过 API 读取和更新。

重置功能支持两种模式:快速重置(仅删除数据库和配置文件)和完全重置(同时删除提示词目录),两种模式都能正确处理新旧两种文件布局的向后兼容。

## 依赖关系

### 导入依赖

| 模块 | 说明 |
|------|------|
| `fastapi` | `APIRouter`, `HTTPException` |
| `server.schemas` | 项目相关的 Pydantic 模型 (7个) |
| `registry` | 项目注册表操作函数 (延迟导入) |
| `progress` | `count_passing_tests` 进度统计 (延迟导入) |
| `prompts` | `get_project_prompts_dir`, `scaffold_project_prompts` (延迟导入) |
| `start` | `check_spec_exists` (延迟导入) |
| `autoforge_paths` | 路径辅助函数 (延迟导入) |
| `server.routers.filesystem` | `is_path_blocked` 路径安全检查 |
| `api.database` | `dispose_engine` 数据库引擎释放 |
| `server.services.assistant_database` | `dispose_engine` 助手数据库引擎释放 |

### 被依赖

| 模块 | 引用内容 |
|------|----------|
| `server/routers/__init__.py` | `router` 导出为 `projects_router` |
| `server/main.py` | 通过 `projects_router` 注册到 FastAPI 应用 |

## 关键类/函数

### `validate_project_name(name: str) -> str`
- **参数**: `name` - 项目名称
- **返回值**: 验证后的名称
- **说明**: 使用正则 `^[a-zA-Z0-9_-]{1,50}$` 校验项目名称,防止路径遍历攻击。

### `get_project_stats(project_dir: Path) -> ProjectStats`
- **参数**: `project_dir` - 项目目录路径
- **返回值**: `ProjectStats` 统计信息
- **说明**: 查询项目的通过/进行中/总计测试数量和百分比。

### `list_projects()`
- **路由**: `GET /api/projects`
- **返回值**: `list[ProjectSummary]`
- **说明**: 列出所有注册项目,跳过路径已失效的项目。

### `create_project(project: ProjectCreate)`
- **路由**: `POST /api/projects`
- **返回值**: `ProjectSummary`
- **说明**: 创建新项目。依次检查名称是否已注册、路径是否已注册(支持 Windows 大小写不敏感比较)、路径是否在敏感目录中。创建目录、脚手架提示词文件、注册到注册表。

### `get_project(name: str)`
- **路由**: `GET /api/projects/{name}`
- **返回值**: `ProjectDetail`
- **说明**: 获取项目详细信息,包括提示词目录路径和默认并发数。

### `delete_project(name: str, delete_files: bool)`
- **路由**: `DELETE /api/projects/{name}`
- **参数**: `delete_files` - 是否同时删除文件
- **说明**: 从注册表中删除项目。检查代理是否运行,可选删除项目文件。

### `get_project_prompts(name: str)` / `update_project_prompts(name: str, prompts)`
- **路由**: `GET /api/projects/{name}/prompts`, `PUT /api/projects/{name}/prompts`
- **说明**: 读取/更新项目的三个提示词文件(app_spec.txt, initializer_prompt.md, coding_prompt.md)。

### `reset_project(name: str, full_reset: bool)`
- **路由**: `POST /api/projects/{name}/reset`
- **参数**: `full_reset` - 是否完全重置
- **说明**: 重置项目状态。快速重置删除 features.db、assistant.db 及配置文件;完全重置额外删除 prompts 目录。处理新旧两种文件布局,在删除前释放数据库引擎避免 Windows 文件锁问题。

### `update_project_settings(name: str, settings: ProjectSettingsUpdate)`
- **路由**: `PATCH /api/projects/{name}/settings`
- **返回值**: `ProjectDetail`
- **说明**: 更新项目级设置(如默认并发数)。

## 架构图

```mermaid
graph TD
    A[React UI] -->|REST API| B[projects.py Router]

    B -->|GET /api/projects| C[list_projects]
    B -->|POST /api/projects| D[create_project]
    B -->|GET /{name}| E[get_project]
    B -->|DELETE /{name}| F[delete_project]
    B -->|POST /{name}/reset| G[reset_project]
    B -->|GET /{name}/prompts| H[get_project_prompts]
    B -->|PUT /{name}/prompts| I[update_project_prompts]
    B -->|PATCH /{name}/settings| J[update_project_settings]

    C --> K[registry.list_registered_projects]
    D --> L[registry.register_project]
    D --> M[prompts.scaffold_project_prompts]
    D --> N[filesystem.is_path_blocked]
    F --> O[registry.unregister_project]
    G --> P[autoforge_paths.*]
    G --> Q[api.database.dispose_engine]
```

## 注意事项

1. **延迟导入模式**: 所有外部模块(registry, progress, prompts, start)都使用延迟导入以避免循环依赖。`_init_imports()` 确保模块仅初始化一次。
2. **路径安全**: 创建项目时使用 `filesystem.is_path_blocked` 检查目标路径是否在敏感目录中(如 `.ssh`, `.aws` 等)。
3. **Windows 兼容**: 项目路径重复检查在 Windows 上使用大小写不敏感比较;重置时先释放 SQLite 数据库引擎以解除文件锁。
4. **向后兼容**: 重置操作同时清理新布局 (`.autoforge/`) 和旧布局 (项目根目录) 的文件,确保迁移前的项目也能正确重置。
5. **代理运行检查**: 删除和重置操作前通过 `has_agent_running()` 检查是否有代理正在运行,防止数据竞争。
