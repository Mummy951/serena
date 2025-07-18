# MCP 安装说明

本文档主要用作对 AI 助手（如 Cline 等）的说明，这些助手尝试根据自由格式的说明进行自动安装。

0.  确保已安装 `uv`。如果未安装，请使用 `curl -LsSf https://astral.sh/uv/install.sh | sh` (macOS, Linux) 或 `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"` (Windows) 进行安装。找到 `uv` 可执行文件的路径，稍后您将需要它。
1.  使用 `git clone git@github.com:oraios/serena.git` 克隆仓库并进入其目录（例如，`cd serena`）
2.  检查 `serena_config.yml` 是否存在。如果不存在，请使用 `cp serena_config.template.yml serena_config.yml` 创建它。阅读配置中的说明。
3.  在配置中，检查是否已添加您的项目路径。如果没有，请将其添加到 `projects` 部分。
4.  在您的项目中，如果需要，创建 `.serena` 目录，并检查 `project.yml` 是否存在。
5.  如果未找到 `project.yml`，请使用 `cp /path/to/serena/myproject.template.yml /path/to/your/project/.serena/project.yml` 创建它。
6.  阅读 `project.yml` 中的说明。确保 `project.yml` 已配置正确的项目语言。删除其中的 `project_root` 条目。
7.  最后，添加 Serena MCP 服务器配置，如下所示：

```json
   {
       "mcpServers": {
            ...
           "serena": {
               "command": "/abs/path/to/uv",
               "args": ["run", "--directory", "/abs/path/to/serena", "serena-mcp-server", "/path/to/your/project/.serena/project.yml"]
           }
       }
   }

``` 