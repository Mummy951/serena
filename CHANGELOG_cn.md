# 最新进展

`main` 分支的状态。下一次正式版本发布之前的更改将显示在此处。

*   **最大限度地减少 `asyncio` 的使用**，提高稳定性并减少对变通方案的需求
    *   切换到新开发的完全同步 LSP 库 `solidlsp`（源自 `multilspy`），移除我们的 `multilspy` 分支 (`src/multilspy`)
    *   将 Serena dashboard 中使用的 fastapi（使用 asyncio）切换到 Flask
    *   MCP 服务器现在是唯一的基于 asyncio 的组件，解决了跨组件的循环污染问题，因此不再需要进程隔离。
        Windows 上也不再需要非正常关机。
*   **改进的编辑工具**：编辑逻辑得到简化和改进，使其更健壮。
    *   删除了“最小缩进”逻辑，因为 LLM 不理解它。
    *   改进了空行插入逻辑（现在主要由 LLM 控制）
*   为代理添加一个任务队列，该队列在单独的线程中执行，并且
    *   允许语言服务器在后台初始化，使 MCP 服务器在启动时立即响应请求，
    *   确保所有工具执行完全同步（线性执行）。
*   `SearchForPatternTool`：更好的默认设置，扩展了限制搜索的参数和描述
*   语言支持：
    *   通过从 `omnisharp` 切换到 Microsoft 官方的 C# 语言服务器，更好地支持 C#。
    *   **添加对 Clojure 的支持**
*   配置：
    *   添加选项 `web_dashboard_open_on_launch`（允许启用仪表板而不打开浏览器窗口）
    *   添加选项 `record_tool_usage_stats` 和 `token_count_estimator`
*   仪表板：
    *   如果配置中启用，则显示工具使用统计信息

修复：
*   修复 `ExecuteShellCommandTool` 和 `GetCurrentConfigTool` 在 Windows 上挂起的问题
*   修复通过 `--project` 激活项目名称不起作用的问题（在之前版本中已损坏）
*   改进符号编辑工具中缩进和换行的处理
*   修复 `InsertAfterSymbolTool` 在文件末尾插入时没有以换行符结尾而失败的问题
*   修复 `InsertBeforeSymbolTool` 在引用符号上方没有空行时插入位置错误的问题
*   修复 `ReplaceSymbolBodyTool` 更改符号前后空白的问题
*   修复存储库索引不遵循链接并在索引期间捕获异常，允许即使单个文件发生意外错误也能继续索引。
*   修复 Ruby 语言服务器中的 `ImportError`。
*   修复 `search_for_pattern` 工具中 gitignore 匹配和正则表达式解释的一些问题。

# 2025-06-20

*   **编辑工具大修和重大改进！**
    这代表了 Serena 的一个非常重要的变化。符号现在可以通过其 `name_path`（包括嵌套符号）进行寻址，并且我们引入了基于正则表达式的替换工具。我们调整了提示并测试了新的编辑机制。
    它更可靠、更灵活，同时使用的令牌更少。
    行替换工具默认禁用并已弃用，我们可能会很快将其移除。
*   **更好的多项目支持和零配置设置**：我们显著简化了配置设置，您不再需要手动为每个项目创建 `project.yaml`。项目激活现在始终可用。
    现在只需要求 LLM 激活任何项目并传递存储库的路径即可。
*   仪表板作为 Web 应用程序以及从仪表板（或旧日志 GUI）关闭 Serena 的可能性。
*   可以预先索引项目，从而加速 Serena 的工具。
*   支持项目的初始提示（目前需要手动添加）
*   模式搜索工具的性能大幅提升
*   使用**进程隔离**来修复稳定性问题和死锁（参见 #170）。
    这为 MCP 服务器、Serena 代理和仪表板使用单独的进程，以解决与 asyncio 相关的问题。

# 2025-05-24

*   重要新功能：**模式和上下文的可配置性**，允许更好地集成到各种客户端中。
    请参阅自述文件中的相应部分——Serena 现在可以更高效地集成到 IDE 助手中。
    您现在还可以执行以下操作：切换到一次性规划模式，要求规划一些内容（这将创建记忆），然后在下一次对话中切换到交互式编辑模式，并处理从记忆中读取的计划。
*   提示的一些改进。

# 2025-05-21

**符号查找的重大改进！**

*   Serena 核心：
    *   `FindSymbolTool` 现在可以通过指定路径而不是仅符号名称来查找符号
*   语言服务器：
    *   修复了 `gopls` 初始化
    *   通过符号树或概述方法检索的符号现在与其父符号链接


# 2025-05-19

*   Serena 核心：
    *   `FindSymbolTool` 中的错误修复（LS 中修复了一个错误）
    *   `ListDirTool` 中的修复：不要忽略语言服务器无法理解的带扩展名的文件，只跳过被忽略的目录（上一个版本中引入的错误）
    *   将两个概述工具（用于目录和文件）合并为一个：`GetSymbolsOverviewTool`
    *   为 Cline 启用一键设置
    *   `SearchForPatternTool` 现在可以（可选地）在整个项目中搜索
    *   新工具 `RestartLanguageServerTool` 用于重新启动语言服务器（以防 Serena 之外的其他编辑源）
    *   修复 `CheckOnboardingPerformedTool`：
        *   工具描述与项目更改不兼容
        *   返回的结果不如预期的有用（现在添加了记忆列表）

*   语言服务器：
    *   添加 Python (.pyi)、JavaScript (.jsx) 和 TypeScript (.tsx, .jsx) 的语言服务器考虑的其他文件扩展名
    *   更新了 multilspy，增加了对 Kotlin、Dart 和 C/C++ 的支持以及多项改进。
    *   增加了对 PHP 的支持
    

# 2025-04-07

> **重大配置更改**：请确保在项目配置中设置 `ignore_all_files_in_gitignore`，删除 `ignore_dirs` 并（可选地）设置 `ignore_paths`。请参阅[更新后的配置模板](myproject.template.yml)

*   Serena 核心：
    *   新工具：FindReferencingCodeSnippets
    *   调整了 CreateTextFileTool 中的提示，以防止写入部分内容（参见[此处](https://www.reddit.com/r/ClaudeAI/comments/1jpavtm/comment/mloek1x/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)）。
    *   FindSymbolTool：允许传递文件以限制搜索，而不仅仅是目录（Gemini 太笨了，无法传递目录）
    *   原生支持 gitignore 文件，用于配置 Serena 忽略的文件。另请参阅下面的“语言服务器”部分。
    *   **主要功能**：允许 Serena 在项目之间切换（项目激活）
        *   在 `serena_config.yml` 中添加中心 Serena 配置，其中
            *   包含可用项目列表
            *   允许配置是否启用项目激活
            *   现在包含 GUI 日志配置（项目配置不再包含）
        *   添加新工具 `activate_project` 和 `get_active_project`
        *   现在启动参数中提供项目配置文件是可选的
*   日志记录：
    *   改进初始化失败时的错误报告：打开新的 GUI 日志窗口显示错误，或确保现有日志窗口可见一段时间
*   语言服务器：
    *   修复项目路径包含空格时 C# 语言服务器初始化问题
    *   在概述、文档树和 find_references 操作中原生支持 gitignore。
        这是一个**重要**的补充，因为以前会扫描 `venv` 和 `node_modules` 等内容，这可能是导致工具缓慢甚至服务器崩溃（大概是由于内存不足错误）的原因。
*   Agno：
    *   修复 Agno 重新加载机制导致初始化 sqlite 内存数据库失败的问题 #8
    *   修复 Serena GUI 日志窗口在初始化后无法捕获日志的问题

# 2025-04-01

首次公开版本 