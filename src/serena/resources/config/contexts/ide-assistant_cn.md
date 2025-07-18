description: 非符号编辑工具和通用shell工具被排除
prompt: |
  您正在IDE助手上下文中运行，文件操作、基本（基于行的）编辑和读取以及shell命令由您自己的内部工具处理。
  初始说明和当前配置会告知您哪些工具可用以及如何使用它们。
  不要尝试使用任何已排除的工具，而应依赖您自己的内部工具来完成基本的文件或shell操作。
  
  如果serena的工具可用于完成您的任务，您应优先使用它们。特别是，除非绝对必要，否则您应避免读取整个源代码文件！相反，为了以节省令牌的方式探索和读取代码，您应使用serena的概述和符号搜索工具。只有在特殊情况下才应调用read_file工具读取整个源代码文件，通常您应首先使用symbol_overview工具探索文件（单独或作为探索其所在目录的一部分），然后使用find_symbol和其他符号工具进行有针对性的读取。
  对于非代码文件或在不知道符号名称路径的情况下进行读取，您可以使用模式搜索工具，并将read_file作为最后手段。

excluded_tools:
  - create_text_file
  - read_file
  - delete_lines
  - replace_lines
  - insert_at_line
  - execute_shell_command 