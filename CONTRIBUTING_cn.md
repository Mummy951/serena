# 贡献 Serena

Serena 正在积极开发中。我们正在探索它的功能和局限性。

欢迎通过提出新问题、功能请求和扩展来分享您的学习。

## 开发者环境设置

您可以通过 `uv` 或基于 Docker 解释器进行本地设置。该存储库也配置为在 GitHub Codespace 中无缝工作。请参阅下面各种设置场景的说明。

无论设置如何完成，虚拟环境都可以通过 `uv` 创建和激活（见下文），并且可以使用 `poe` 执行各种任务，例如格式化、测试和文档构建。例如，`poe format` 将格式化代码，包括笔记本。只需运行 `poe` 即可查看可用命令。

### Python (uv) 设置

您可以通过以下方式安装具有所需依赖项的虚拟环境：

1.  创建一个新的虚拟环境：`uv venv`
2.  激活环境：
    *   在 Linux/Unix/macOS 上：`source .venv/bin/activate`
    *   在 Windows 上：`.venv\Scripts\activate.bat` (在 cmd/ps 中) 或 `source .venv/Scripts/activate` (在 git-bash 中)
3.  安装所有必需的包和所有额外项：`uv pip install --all-extras -r pyproject.toml -e .`

### Docker 设置

使用以下命令构建 Docker 镜像：

```shell
docker build -t serena .
```

并使用存储库作为卷运行它：

```shell
docker run -it --rm -v "$(pwd)":/workspace serena
```

您也可以直接运行 `bash docker_build_and_run.sh`，它将为您完成这两项工作。

注意：对于适用于 Linux 的 Windows 子系统 (WSL)，您可能需要调整卷的路径。

## 本地运行工具

Serena 工具（实际上所有 Serena 代码）都可以在没有 LLM 的情况下执行，也可以在没有任何 MCP 特殊功能的情况下执行（尽管如果您愿意，可以使用 mcp 检查器）。

[scripts/demo_run_tools.py](scripts/demo_run_tools.py) 中提供了一个运行工具的示例脚本。

## 添加新的支持语言

Serena 通过 `solidlsp` 包中包含的语言服务器与代码交互。如果存在 LSP 实现，则添加新的支持语言非常容易。您只需：

1.  创建一个新的 `SolidLanguageServer` 子类
2.  向 `Language` 枚举添加一个新值
3.  在 `SolidLanguageServer.create` 方法中添加一个新的 `elif` 情况
4.  将新语言的测试存储库添加到 `test/resources/repos/<new_language>/test_repo` 中
    并在 `test/solidlsp/<new_language>` 中添加新的测试。类似于其他语言的现有测试
5.  还在 `test/serena/test_serena_agent` 中的参数化测试中添加一个新案例

子类通常很容易编写，请参阅 [PyrightLanguageServer](src/multilspy/language_servers/pyright_language_server/pyright_server.py) 作为示例，或参阅任何其他实现以了解如何在其中处理语言服务器的非 Python 依赖项。
multilspy 管理员[此处](https://github.com/microsoft/multilspy/issues/5)也有一些提示。 