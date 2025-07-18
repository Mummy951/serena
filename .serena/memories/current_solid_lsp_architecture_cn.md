# 当前架构：Solid-LSP 实现

## 概述

Solid-LSP (`src/solidlsp/`) 是 Serena 当前的语言服务器集成层，设计为一个**简化的、抗死锁的**替代方案，用于替换有问题的 multilspy 架构。它支持与 MCP 服务器的**单进程操作**，同时保持所有语义代码分析功能。

## 当前系统状态

- **默认实现**：Solid-LSP 是唯一的语言服务器实现（multilspy 已完全移除）
- **进程隔离**：**默认禁用**（`USE_PROCESS_ISOLATION = False`）
- **MCP 集成**：在与 MCP 服务器相同的进程中安全运行
- **性能**：比进程隔离的 multilspy 具有更低的延迟和资源使用

## 核心架构

### 主要组件

#### 1. **`SolidLanguageServer`** (`src/solidlsp/ls.py`)
- **1,634 行**：主要语言服务器接口
- **增强方法**：与 multilspy 相同的语义功能，但具有改进的异步处理
- **生命周期控制**：`start()`、`stop()`、`is_running()`、`language_server()` 属性
- **清洁的异步模式**：异步上下文之间没有协程泄漏

#### 2. **`SolidLanguageServerHandler`** (`src/solidlsp/ls_handler.py`)
- **512 行**：简化的 LSP 协议处理器
- **直接进程管理**：比 multilspy 复杂的协调更直接
- **资源管理**：更好的清理和进程终止

#### 3. **协议层** (`src/solidlsp/lsp_protocol_handler/`)
- **简化协议**：直接的 LSP 通信，没有过度抽象
- **类型和常量**：LSP 类型定义和协议常量
- **请求处理**：简化的请求/响应模式

### 语言服务器支持

Solid-LSP 维护与 multilspy **相同的语言支持**：

#### 主要语言（直接支持）
- **Python**：Pyright 语言服务器 (`src/solidlsp/language_servers/pyright_language_server/`)
- **Java**：Eclipse JDTLS (`src/solidlsp/language_servers/eclipse_jdtls/`)
- **TypeScript/JavaScript**：TypeScript 语言服务器 (`src/solidlsp/language_servers/typescript_language_server/`)

#### 其他语言（完全支持）
- **C#**：OmniSharp (`src/solidlsp/language_servers/omnisharp/`)
- **Rust**：Rust-Analyzer (`src/solidlsp/language_servers/rust_analyzer/`)
- **Go**：Gopls (`src/solidlsp/language_servers/gopls/`)
- **Ruby**：Solargraph (`src/solidlsp/language_servers/solargraph/`)
- **C++**：Clangd (`src/solidlsp/language_servers/clangd_language_server/`)
- **Dart**：Dart 语言服务器 (`src/solidlsp/language_servers/dart_language_server/`)
- **PHP**：Intelephense (`src/solidlsp/language_servers/intelephense/`)
- **Kotlin**：Kotlin 语言服务器 (`src/solidlsp/language_servers/kotlin_language_server/`)

## 关键架构改进

### 1. **简化的异步模式**
- **清晰边界**：适当的异步上下文管理防止 MCP 服务器污染
- **无协程泄漏**：消除了 multilspy 中的未等待协程警告
- **直接操作**：更少的抽象层可能损坏异步上下文

### 2. **单进程安全**
- **安全的 MCP 集成**：设计为在 MCP 服务器进程内工作而不产生死锁
- **消除 IPC 开销**：直接方法调用而不是进程间通信
- **资源效率**：更低的内存使用和更快的启动时间

### 3. **增强的进程控制**
- **直接进程管理**：`_start_server_process()`、`_start_server()` 方法
- **更好的状态管理**：用于增强控制的 `_server_context` 属性
- **改进的清理**：更可靠的资源管理和进程终止

## 集成点

### MCP 服务器集成
`src/serena/mcp.py:590` 中的当前实现：
```python
if not USE_PROCESS_ISOLATION:
    mcp_factory = SerenaMCPFactorySingleProcess(context=context, project=project_file)
else:
    mcp_factory = SerenaMCPFactoryWithProcessIsolation(context=context, project=project_file)
```

### 代理集成
`src/serena/agent.py:642-689` 中的语言服务器创建：
```python
def create_ls_for_project(...) -> SolidLanguageServer:
    # 使用项目设置创建 LanguageServerConfig
    # 返回 SolidLanguageServer.create() 实例
```

## 配置和设置

### 核心配置
- **`USE_PROCESS_ISOLATION = False`**：默认禁用进程隔离
- **语言检测**：基于项目组成的自动检测
- **超时设置**：可配置的语言服务器超时
- **LSP 通信跟踪**：可选的调试支持

### 项目级设置
- **忽略路径**：尊重项目配置和 gitignore
- **语言选择**：自动或手动语言服务器选择
- **超时配置**：每个项目的超时设置

## 性能优势

### 与 Multilspy + 进程隔离相比
1. **更低延迟**：直接方法调用而不是 IPC
2. **减少内存**：单进程而不是多进程
3. **更快启动**：没有进程创建开销
4. **更简单调试**：所有组件在同一进程中，具有统一的堆栈跟踪

### 稳定性改进
1. **无 Asyncio 死锁**：清晰的异步边界防止污染
2. **可靠操作**：`find_symbol` 和其他工具始终正常工作
3. **资源管理**：更好的清理防止资源泄漏
4. **错误处理**：简化的错误传播和处理

## 当前运行状态

- **生产就绪**：所有 Serena 部署的默认实现
- **完全测试**：所有语义工具（`find_symbol`、`replace_symbol_body` 等）可靠工作
- **MCP 兼容**：与 Claude Desktop 和其他 MCP 客户端稳定运行
- **跨平台**：在所有支持的操作系统上工作

这个架构代表了成功解决 multilspy asyncio 污染问题的方案，同时保持了完整的语义代码分析功能并改进了整体性能。 