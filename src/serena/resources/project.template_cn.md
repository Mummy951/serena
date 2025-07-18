# 项目语言 (csharp, python, rust, java, typescript, go, cpp, 或 ruby)
# * 对于 C 语言，请使用 cpp
# * 对于 JavaScript，请使用 typescript
# 特殊要求:
# * csharp: 要求项目文件夹中存在 .sln 文件。
language: python

# 是否使用项目的 .gitignore 文件来忽略文件
# 于 2025-04-07 添加
ignore_all_files_in_gitignore: true
# 额外的忽略路径列表
# 语法与 .gitignore 相同，可以使用 * 和 **
# 之前名为 `ignored_dirs`，如果您正在使用，请更新您的配置。
# 于 2025-04-07 添加（重命名）
ignored_paths: []

# 项目是否处于只读模式
# 如果设置为 true，所有编辑工具将被禁用，尝试使用它们将导致错误。
# 于 2025-04-18 添加
read_only: false


# 要排除的工具名称列表。我们建议不要排除任何工具，更多详细信息请参阅 README。
# 下面是为了方便而提供的完整工具列表。
# 为确保您拥有最新的工具列表并查看其描述，
# 请执行 `uv run scripts/print_tool_overview.py`。
#
# * `activate_project`: 通过名称激活项目。
# * `check_onboarding_performed`: 检查是否已执行项目引导。
# * `create_text_file`: 在项目目录中创建/覆盖文件。
# * `delete_lines`: 删除文件中指定范围的行。
# * `delete_memory`: 从 Serena 的项目特定内存存储中删除内存。
# * `execute_shell_command`: 执行 shell 命令。
# * `find_referencing_code_snippets`: 查找引用给定位置符号的代码片段。
# * `find_referencing_symbols`: 查找引用给定位置符号的符号（可选按类型过滤）。
# * `find_symbol`: 对具有/包含给定名称/子字符串的符号执行全局（或局部）搜索（可选按类型过滤）。
# * `get_current_config`: 打印代理的当前配置，包括活动和可用的项目、工具、上下文和模式。
# * `get_symbols_overview`: 获取给定文件或目录中定义的顶级符号概览。
# * `initial_instructions`: 获取当前项目的初始指令。
# 仅应在无法设置系统提示的环境中使用，
# 例如在您无法控制的客户端中，如 Claude Desktop。
# * `insert_after_symbol`: 在给定符号定义结束后插入内容。
# * `insert_at_line`: 在文件的给定行插入内容。
# * `insert_before_symbol`: 在给定符号定义开始前插入内容。
# * `list_dir`: 列出给定目录中的文件和目录（可选递归）。
# * `list_memories`: 列出 Serena 项目特定内存存储中的内存。
# * `onboarding`: 执行引导（识别项目结构和基本任务，例如测试或构建）。
# * `prepare_for_new_conversation`: 提供准备新对话的指令（以便继续必要的上下文）。
# * `read_file`: 读取项目目录中的文件。
# * `read_memory`: 从 Serena 项目特定内存存储中读取具有给定名称的内存。
# * `remove_project`: 从 Serena 配置中移除项目。
# * `replace_lines`: 用新内容替换文件中指定范围的行。
# * `replace_symbol_body`: 替换符号的完整定义。
# * `restart_language_server`: 重启语言服务器，当非 Serena 编辑发生时可能需要。
# * `search_for_pattern`: 在项目中执行模式搜索。
# * `summarize_changes`: 提供总结代码库更改的指令。
# * `switch_modes`: 通过提供模式名称列表来激活模式。
# * `think_about_collected_information`: 用于思考收集信息完整性的思考工具。
# * `think_about_task_adherence`: 用于确定代理是否仍在正确执行当前任务的思考工具。
# * `think_about_whether_you_are_done`: 用于确定任务是否真正完成的思考工具。
# * `write_memory`: 写入命名内存（供将来参考）到 Serena 的项目特定内存存储。
excluded_tools: []

# 项目的初始提示。它将在激活项目时始终提供给 LLM
# （与按需加载的内存相反）。
initial_prompt: ""

project_name: "project_name" 