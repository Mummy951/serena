# project.yml 配置文件中文说明

该文件是 Serena 项目的配置文件。

---

### `language`
项目语言（csharp, python, rust, java, typescript, javascript, go, cpp, 或 ruby）。
特殊要求：
* `csharp`: 需要项目文件夹中存在 `.sln` 文件。

**当前设置:** `python`

---

### `ignore_all_files_in_gitignore`
是否使用项目的 `.gitignore` 文件来忽略文件。
*添加于 2025-04-07*

**当前设置:** `true`

---

### `ignored_paths`
要忽略的其他路径列表。
语法与 `.gitignore` 相同，因此您可以使用 `*` 和 `**`。
*之前称为 `ignored_dirs`，如果您在使用该名称，请更新您的配置。*
*添加（重命名）于 2025-04-07*

**当前设置:** `[]` (无)

---

### `read_only`
项目是否处于只读模式。
如果设置为 `true`，所有编辑工具将被禁用，任何使用它们的尝试都会导致错误。
*添加于 2025-04-18*

**当前设置:** `false`

---

### `excluded_tools`
要排除的工具名称列表。我们建议不要排除任何工具，更多详细信息请参阅自述文件。
为方便起见，下面是完整的工具列表。
要确保您拥有最新的工具列表并查看其描述，请执行 `uv run scripts/print_tool_overview.py`。

*   `activate_project`: 按名称激活项目。
*   `check_onboarding_performed`: 检查是否已执行项目引导。
*   `create_text_file`: 在项目目录中创建/覆盖文件。
*   `delete_lines`: 删除文件中的一系列行。
*   `delete_memory`: 从 Serena 的项目特定内存存储中删除一个内存。
*   `execute_shell_command`: 执行 shell 命令。
*   `find_referencing_code_snippets`: 查找引用给定位置符号的代码片段。
*   `find_referencing_symbols`: 查找引用给定位置符号的符号（可选地按类型过滤）。
*   `find_symbol`: 对具有/包含给定名称/子字符串的符号执行全局（或本地）搜索（可选地按类型过滤）。
*   `get_current_config`: 打印代理的当前配置，包括活动和可用的项目、工具、上下文和模式。
*   `get_symbols_overview`: 获取给定文件或目录中定义的顶级符号的概述。
*   `initial_instructions`: 获取当前项目的初始指令。
    仅应在无法设置系统提示的环境中使用，例如，在您无法控制的客户端（如 Claude Desktop）中。
*   `insert_after_symbol`: 在给定符号定义的末尾插入内容。
*   `insert_at_line`: 在文件中的给定行插入内容。
*   `insert_before_symbol`: 在给定符号定义的开头插入内容。
*   `list_dir`: 列出给定目录中的文件和目录（可选地递归）。
*   `list_memories`: 列出 Serena 的项目特定内存存储中的内存。
*   `onboarding`: 执行引导（识别项目结构和基本任务，例如测试或构建）。
*   `prepare_for_new_conversation`: 提供准备新对话的说明（以便在必要上下文中继续）。
*   `read_file`: 读取项目目录中的文件。
*   `read_memory`: 从 Serena 的项目特定内存存储中读取具有给定名称的内存。
*   `remove_project`: 从 Serena 配置中删除一个项目。
*   `replace_lines`: 用新内容替换文件中的一系列行。
*   `replace_symbol_body`: 替换符号的完整定义。
*   `restart_language_server`: 重新启动语言服务器，当发生非 Serena 的编辑时可能需要。
*   `search_for_pattern`: 在项目中搜索模式。
*   `summarize_changes`: 提供总结对代码库所做更改的说明。
*   `switch_modes`: 通过提供模式名称列表来激活模式。
*   `think_about_collected_information`: 用于思考所收集信息完整性的思维工具。
*   `think_about_task_adherence`: 用于确定代理是否仍在当前任务轨道上的思维工具。
*   `think_about_whether_you_are_done`: 用于确定任务是否真正完成的思维工具。
*   `write_memory`: 将命名内存（供将来参考）写入 Serena 的项目特定内存存储。

**当前设置:** `[]` (无)

---

### `initial_prompt`
项目的初始提示。激活项目时，它将始终提供给 LLM（与按需加载的内存相反）。

**当前设置:** `""` (空)

---

### `project_name`
项目名称。

**当前设置:** `"serena"` 