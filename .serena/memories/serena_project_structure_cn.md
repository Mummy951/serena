# Serena 项目结构概览

## 顶层组织

Serena 组织成几个关键目录，每个目录都有不同的架构目的：

```
serena/
├── src/                    # 主要源代码
│   ├── interprompt/        # 模板和提示管理系统
│   ├── serena/            # 核心 Serena 功能
│   └── solidlsp/          # 语言服务器集成层
├── tests/                 # 测试套件
├── docs/                  # 文档
├── prompts/               # 提示模板和配置
├── contexts/              # 上下文定义（desktop-app、agent、ide-assistant）
├── modes/                 # 模式定义（planning、editing、interactive、one-shot）
└── pyproject.toml         # 项目配置和依赖
```

## 核心源代码结构（`src/`）

### 1. **Serena 核心（`src/serena/`）**
Serena 系统的心脏，包含：

#### **代理和核心逻辑**
- **`agent.py`**：主要的 `SerenaAgent` 类、工具实现、项目管理
- **`mcp.py`**：模型上下文协议服务器实现和 MCP 工厂
- **`config.py`**：配置系统（上下文、模式、注册配置）
- **`constants.py`**：系统常量，包括 `USE_PROCESS_ISOLATION = False`

#### **专门组件**
- **`dashboard.py`**：用于监控和日志记录的 Web 仪表板（`MemoryLogHandler`、`SerenaDashboardAPI`）
- **`symbol.py`**：符号管理（`SymbolLocation`、`Symbol`、`SymbolManager`）
- **`text_utils.py`**：用于搜索和文件操作的文本处理工具
- **`gui_log_viewer.py`**：GUI 日志窗口实现

#### **集成层**
- **`agno.py`**：Agno 框架集成（`SerenaAgnoToolkit`、`SerenaAgnoAgentProvider`）
- **`process_isolated_agent.py`**：进程隔离基础设施（可用但默认未使用）
- **`prompt_factory.py`**：提示生成和管理

#### **实用模块（`src/serena/util/`）**
- **`file_system.py`**：文件扫描、gitignore 解析（`GitignoreParser`、`scan_directory`）
- **`git.py`**：Git 操作和状态检查
- **`shell.py`**：Shell 命令执行（`execute_shell_command`）
- **`thread.py`**：带超时支持的线程工具
- **`inspection.py`**：代码检查和语言检测工具

### 2. **Solid-LSP（`src/solidlsp/`）**
当前的语言服务器集成层：

#### **核心语言服务器组件**
- **`ls.py`**：主要的 `SolidLanguageServer` 类（1,634 行）
- **`ls_handler.py`**：用于协议管理的 `SolidLanguageServerHandler`（512 行）
- **`ls_request.py`**：请求处理（`LanguageServerRequest`）
- **`ls_types.py`**：LSP 类型定义
- **`ls_utils.py`**：实用类（`TextUtils`、`PathUtils`、`FileUtils`、`SymbolUtils`）

#### **协议处理（`src/solidlsp/lsp_protocol_handler/`）**
- **`server.py`**：LSP 协议服务器实现
- **`lsp_requests.py`**：LSP 请求和通知类
- **`lsp_types.py`**：完整的 LSP 类型系统定义
- **`lsp_constants.py`**：LSP 协议常量

#### **语言服务器实现（`src/solidlsp/language_servers/`）**
每种语言都有自己的子目录和特定实现：
- **`pyright_language_server/`**：通过 Pyright 支持 Python
- **`eclipse_jdtls/`**：通过 Eclipse JDTLS 支持 Java
- **`typescript_language_server/`**：TypeScript/JavaScript 支持
- **`omnisharp/`**：通过 OmniSharp 支持 C#
- **`rust_analyzer/`**：通过 Rust-Analyzer 支持 Rust
- **`gopls/`**：通过 Gopls 支持 Go
- **`solargraph/`**：通过 Solargraph 支持 Ruby
- **`clangd_language_server/`**：通过 Clangd 支持 C++
- **`dart_language_server/`**：Dart 支持
- **`intelephense/`**：通过 Intelephense 支持 PHP
- **`kotlin_language_server/`**：Kotlin 支持

### 3. **Interprompt（`src/interprompt/`）**
模板和提示管理系统：

- **`multilang_prompt.py`**：多语言提示模板（`MultiLangPromptTemplate`）
- **`jinja_template.py`**：Jinja2 模板集成（`JinjaTemplate`）
- **`prompt_factory.py`**：提示工厂基类
- **`util/class_decorators.py`**：实用装饰器如 `@singleton`

## 配置和模板

### **提示模板（`prompts/`）**
按上下文和功能组织：
- **上下文特定提示**：不同执行上下文的不同提示集
- **工具特定提示**：特定工具和操作的专门提示
- **多语言支持**：在适用时提供多种语言的提示

### **上下文（`contexts/`）**
执行环境定义：
- **`desktop-app.yml`**：桌面应用程序上下文
- **`agent.yml`**：代理特定上下文
- **`ide-assistant.yml`**：IDE 助手上下文
- **自定义上下文**：支持用户定义的上下文

### **模式（`modes/`）**
行为模式定义：
- **`planning.yml`**：规划模式行为
- **`editing.yml`**：编辑模式行为
- **`interactive.yml`**：交互模式行为
- **`one-shot.yml`**：一次性执行模式

## 关键架构模式

### 1. **四层配置层次结构**
1. **全局**：`serena_config.yml`
2. **CLI 参数**：运行时覆盖
3. **项目**：`.serena/project.yml`
4. **活动模式**：运行时行为修改

### 2. **工具系统架构**
- **基类**：`agent.py` 中的 `ToolInterface`、`Tool`
- **标记接口**：`ToolMarkerCanEdit`、`ToolMarkerDoesNotRequireActiveProject`
- **工具注册表**：通过 `_iter_tool_classes()` 自动工具发现的 `ToolRegistry`
- **工具类别**：语义工具、文件操作、项目管理、元工具

### 3. **集成模式**
- **MCP 服务器**：通过 `SerenaMCPFactory` 类的主要集成
- **Agno 代理**：通过 `SerenaAgnoToolkit` 的模型无关集成
- **进程隔离**：通过 `ProcessIsolatedSerenaAgent` 的可选隔离

### 4. **内存管理**
- **项目内存**：`.serena/memories/` 目录用于项目特定信息
- **内存工具**：`WriteMemoryTool`、`ReadMemoryTool`、`ListMemoriesTool`
- **内存管理器**：`MemoriesManager`、`MemoriesManagerMDFilesInProject`

## 依赖和构建系统

### **开发工具**
- **`uv`**：依赖管理和虚拟环境
- **`poe`**：任务编排（在 `pyproject.toml` 中定义）
- **基本命令**：`uv run poe lint`、`uv run poe format`、`uv run poe type-check`、`uv run poe test`

### **关键依赖**
- **语言服务器协议**：每种支持语言的 LSP 实现
- **MCP（模型上下文协议）**：用于与 Claude Desktop 和其他 MCP 客户端集成
- **Agno 框架**：用于模型无关的代理实现
- **AsyncIO**：用于并发操作（仔细管理以避免污染）

## 项目状态管理

### **项目配置**
- **全局配置**：用户主目录中的 `serena_config.yml`
- **项目配置**：项目根目录中的 `.serena/project.yml`
- **自动生成**：缺少时自动创建默认配置

### **语言检测**
- **自动**：基于文件组成分析
- **手动覆盖**：通过项目配置
- **多语言**：支持多种语言的项目

## 运行时架构

### **默认操作模式**
- **单进程**：MCP 服务器、代理和语言服务器在同一进程中
- **直接通信**：无 IPC 开销
- **Solid-LSP**：唯一的语言服务器实现
- **进程隔离**：默认禁用（`USE_PROCESS_ISOLATION = False`）

### **语义工具集成**
- **基于符号的操作**：`find_symbol`、`replace_symbol_body`、`insert_after_symbol`
- **基于正则表达式的操作**：`replace_regex` 用于细粒度编辑
- **文件操作**：`read_file`、`create_text_file`、`list_dir`
- **项目操作**：`search_for_pattern`、`get_symbols_overview`

这个结构反映了 Serena 从复杂的多进程系统演进到简化的单进程架构，在保持所有语义功能的同时提高了性能和可靠性。 