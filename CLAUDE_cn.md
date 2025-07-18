# CLAUDE.md

此文件为 Claude Code (claude.ai/code) 在处理此存储库中的代码时提供指导。

## 基本命令

本项目使用 `uv` 进行依赖管理，使用 `poe` 进行任务编排：

-   **Linting**: `uv run poe lint` (仅允许的 linting 命令)
-   **格式化**: `uv run poe format` (仅允许的格式化命令)
-   **类型检查**: `uv run poe type-check` (仅允许的类型检查命令)
-   **测试**: `uv run poe test [args]` (首选) 或 `uv run pytest [args]`

**重要**: 在任务结束时始终运行 `format`、`type-check` 和 `test`，以确保代码质量。修复所有问题并重新运行，直到它们通过。

## 架构概述

Serena 是一个强大的编码代理工具包，它将 LLM 转化为功能齐全的代理，通过语义代码检索和编辑工具直接在代码库上工作。

### 核心组件

-   **src/serena/**: 包含核心功能的主包
    -   `mcp.py`: 模型上下文协议服务器实现
    -   `agno.py`: Agno 框架集成，用于与模型无关的代理
    -   `agent.py`: 具有语义工具的核心代理实现
    -   **llm/**: LLM 集成模块
    -   **util/**: 实用函数和辅助工具

-   **src/multilspy/**: 语言服务器集成层
    -   **language_servers/**: 特定语言的实现
        -   Python (pyright/jedi), Java (Eclipse JDTLS), TypeScript/JavaScript
        -   C#, Rust, Go, Ruby, C++, PHP 支持
    -   通过语言服务器协议 (LSP) 提供语义代码分析

-   **src/interprompt/**: 模板和提示管理系统

### 关键设计原则

-   **语义代码理解**: 使用语言服务器 (LSP) 进行符号级代码分析，而不是基于文本的方法
-   **多种集成方法**: 可用作 MCP 服务器、Agno 代理，或集成到自定义框架中
-   **语言无关**: 通过语言服务器适配器支持多种编程语言
-   **上下文和模式系统**: 可配置不同环境（桌面应用、IDE 助手、代理）的行为

### 集成模式

-   **MCP 服务器**: Claude Desktop 和其他 MCP 客户端的主要集成方法
-   **Agno 代理**: 适用于任何支持 GUI 的 LLM 的模型无关代理框架
-   **框架适配器**: 工具可以适应任何代理框架（示例：SerenaAgnoToolkit）

### 配置系统

四层配置层次结构：
1.  `serena_config.yml` - 全局设置
2.  CLI 参数 - 客户端特定覆盖
3.  `.serena/project.yml` - 项目特定设置
4.  活动模式 - 运行时行为修改

代码库通过语言服务器实现复杂的语义代码操作，从而实现精确的符号级编辑和超越简单文本操作的代码理解。

## 使用 Serena 的代码

**重要**: 在使用此代码库时，请记住您正在使用 Serena 自己的工具来改进 Serena 本身。这意味着：

-   您可以使用 Serena 的语义工具 (`find_symbol`、`replace_symbol_body`、`search_for_pattern` 等) 来分析和编辑 Serena 自己的源代码
-   当要求“对 Serena 执行任务”时，您被要求修改/改进当前的 Serena 代码库
-   您可以访问 Serena 语义代码理解的全部能力来处理 Serena 的代码 