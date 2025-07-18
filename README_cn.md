<p align="center" style="text-align:center">
  <img src="resources/serena-logo.svg#gh-light-mode-only" style="width:500px">
  <img src="resources/serena-logo-dark-mode.svg#gh-dark-mode-only" style="width:500px">
</p>

*   :rocket: Serena 是一个强大的**编码代理工具包**，能够将 LLM 转化为直接**在您的代码库上工作**的功能齐全的代理。
*   :wrench: Serena 提供必要的**语义代码检索和编辑工具**，类似于 IDE 的功能，能够在符号级别提取代码实体并利用关系结构。
*   :free: Serena 是**免费和开源的**，免费增强您已访问的 LLM 的功能。

### 演示

这是一个 Serena 使用 Claude Desktop 为自身实现一个小功能（更好的日志 GUI）的演示。
请注意 Serena 的工具如何让 Claude 找到并编辑正确的符号。

https://github.com/user-attachments/assets/6eaa9aa1-610d-4723-a2d6-bf1e487ba753

<p align="center">
  <em>Serena 正在积极开发中！查看最新更新、即将推出的功能和经验总结，以保持更新。</em>
</p>

<p align="center">
  <a href="CHANGELOG.md">
    <img src="https://img.shields.io/badge/Updates-1e293b?style=flat&logo=rss&logoColor=white&labelColor=1e293b" alt="更新日志" />
  </a>
  <a href="roadmap.md">
    <img src="https://img.shields.io/badge/Roadmap-14532d?style=flat&logo=target&logoColor=white&labelColor=14532d" alt="路线图" />
  </a>
  <a href="lessons_learned.md">
    <img src="https://img.shields.io/badge/Lessons-Learned-7c4700?style=flat&logo=readthedocs&logoColor=white&labelColor=7c4700" alt="经验总结" />
  </a>
</p>



### LLM 集成

Serena 为编码工作流提供了必要的[工具](#full-list-of-tools)，但需要 LLM 来完成实际工作，协调工具的使用。

例如，使用[一行 shell 命令](#claude-code)来**大幅提升 Claude Code 的性能**。

Serena 可以通过多种方式与 LLM 集成：
*   通过使用**模型上下文协议 (MCP)**。
    Serena 提供一个 MCP 服务器，它与以下工具集成：
    *   Claude Code 和 Claude Desktop，
    *   IDE，如 VSCode、Cursor 或 IntelliJ，
    *   扩展，如 Cline 或 Roo Code
    *   以及许多其他，包括[即将推出的 ChatGPT 应用程序](https://x.com/OpenAIDevs/status/1904957755829481737)
*   通过使用 **Agno – 模型无关代理框架**。
    Serena 基于 Agno 的代理允许您将几乎任何 LLM 转化为编码代理，无论是 Google、OpenAI 还是 Anthropic（需要付费 API 密钥）提供的模型，还是 Ollama、Together 或 Anyscale 提供的免费模型。
*   通过将 Serena 的工具集成到您选择的代理框架中。
    Serena 的工具实现与框架特定的代码解耦，因此可以轻松适应任何代理框架。

### 编程语言支持和语义分析能力

Serena 的语义代码分析能力建立在**语言服务器**之上，使用广泛实现的语言服务器协议 (LSP)。LSP 提供了一套基于代码符号理解的多功能代码查询和编辑功能。凭借这些能力，Serena 能够像经验丰富的开发人员使用 IDE 功能一样高效地发现和编辑代码。
Serena 即使在非常庞大和复杂的项目中也能高效地找到正确的上下文并做正确的事情！因此，它不仅免费开源，而且经常比现有收费解决方案取得更好的结果。

语言服务器支持广泛的编程语言。
通过 Serena，我们提供
*   直接、开箱即用的支持：
    *   Python
    *   TypeScript/Javascript（目前存在一些不稳定问题，我们正在努力解决）
    *   PHP
    *   Go（需要先安装 go 和 gopls）
    *   Rust
    *   C#（需要安装 dotnet。我们最近切换了底层语言服务器，如果您遇到任何问题，请报告）
    *   Java （*注意*：启动速度慢，首次启动尤其如此。Java 在 macos 和 linux 上可能存在问题，我们正在努力解决。）
    *   Elixir（需要安装 NextLS 和 Elixir；**不支持 Windows** - Next LS 不提供 Windows 二进制文件）
    *   Clojure
    *   C/C++（您可能会遇到查找引用方面的问题，我们正在努力解决）
*   间接支持（可能需要一些代码更改/手动安装）：
    *   Ruby（未经测试）
    *   Kotlin（未经测试）
    *   Dart（未经测试）

    这些语言受语言服务器库支持，但我们尚未明确测试对这些语言的支持是否真正完美无缺。

原则上，通过为新的语言服务器实现提供一个浅层适配器，可以轻松支持更多语言。


## 目录

<!-- Created with markdown-toc -i README.md -->
<!-- Install it with npm install -g markdown-toc -->

<!-- toc -->

-   [我可以用 Serena 做什么？](#我可以用-serena-做什么？)
-   [使用 Serena 的免费编码代理](#使用-serena-的免费编码代理)
-   [快速开始](#快速开始)
    *   [运行 Serena MCP 服务器](#运行-serena-mcp-服务器)
        +   [用法](#用法)
            *   [本地安装](#本地安装)
        -   [使用 uvx](#使用-uvx)
        -   [使用 Docker（实验性）](#使用-docker实验性)
    *   [SSE 模式](#sse-模式)
    *   [命令行参数](#命令行参数)
    *   [配置](#配置)
    *   [项目激活和索引](#项目激活和索引)
    *   [Claude Code](#claude-code-1)
    *   [Claude Desktop](#claude-desktop-1)
    *   [其他 MCP 客户端 (Cline, Roo-Code, Cursor, Windsurf, etc.)](#其他-mcp-客户端-cline-roo-code-cursor-windsurf-etc)
    *   [Agno 代理](#agno-代理)
    *   [其他代理框架](#其他代理框架)
-   [详细用法和建议](#详细用法和建议)
    *   [工具执行](#工具执行)
        +   [Shell 执行和编辑工具](#shell-执行和编辑工具)
    *   [模式和上下文](#模式和上下文)
        +   [上下文](#上下文)
        +   [模式](#模式)
        +   [自定义](#自定义)
    *   [入职培训和记忆](#入职培训和记忆)
    *   [准备您的项目](#准备您的项目)
        +   [构建您的代码库](#构建您的代码库)
        +   [从干净状态开始](#从干净状态开始)
        +   [日志记录、Linting 和自动化测试](#日志记录linting-和自动化测试)
    *   [提示策略](#提示策略)
    *   [代码编辑中的潜在问题](#代码编辑中的潜在问题)
    *   [上下文耗尽](#上下文耗尽)
    *   [将 Serena 与其他 MCP 服务器结合使用](#将-serena-与其他-mcp-服务器结合使用)
    *   [Serena 的日志：仪表板和 GUI 工具](#serena-的日志仪表板和-gui-工具)
    *   [故障排除](#故障排除)
-   [与其他编码代理的比较](#与其他编码代理的比较)
    *   [基于订阅的编码代理](#基于订阅的编码代理)
    *   [基于 API 的编码代理](#基于-api-的编码代理)
    *   [其他基于 MCP 的编码代理](#其他基于-mcp-的编码代理)
-   [致谢](#致谢)
-   [定制和扩展 Serena](#定制和扩展-serena)
-   [工具完整列表](#工具完整列表)

<!-- tocstop -->

## 我可以用 Serena 做什么？

您可以使用 Serena 完成任何编码任务——无论是侧重于分析、规划、设计新组件还是重构现有组件。
由于 Serena 的工具允许 LLM 闭合认知感知-行动循环，因此基于 Serena 的代理可以自主地从头到尾执行编码任务——从初始分析到实现、测试，最后到版本控制系统提交。

Serena 可以读取、写入和执行代码，读取日志和终端输出。
虽然我们不一定鼓励它，“随心所欲编码”当然是可能的，如果您想几乎感觉“代码不再存在”，您可能会发现 Serena 比 IDE 内的代理更适合“随心所欲编码”（因为您将拥有一个独立的 GUI，真正让您忘记）。

## 使用 Serena 的免费编码代理

即使 Anthropic 的 Claude 的免费层也支持 MCP 服务器，因此您可以免费将 Serena 与 Claude 结合使用。
据推测，一旦 ChatGPT Desktop 添加了对 MCP 服务器的支持，很快就可以实现同样的功能。
通过 Agno，您还可以选择将 Serena 与免费/开源模型结合使用。

Serena 是 [Oraios AI](https://oraios-ai.de/) 对开发者社区的贡献。
我们自己也定期使用它。

我们厌倦了不得不支付多个基于 IDE 的订阅（例如 Windsurf 或 Cursor），这些订阅迫使我们在已有的聊天订阅费用之上继续购买令牌。
Claude Code、Cline、Aider 和其他基于 API 的工具所产生的巨大 API 成本同样没有吸引力。
因此，我们构建了 Serena，希望能够取消大多数其他订阅。

## 快速开始

Serena 可以通过多种方式使用，下面您将找到所选集成的说明。

-   如果您只想将 Claude 变成一个免费的编码代理，我们建议通过 [Claude Code](#claude-code-1) 或 [Claude Desktop](#claude-desktop-1) 使用 Serena。
-   如果您想使用 Gemini 或任何其他模型，并且您想要 GUI 体验，您可以使用 [Agno](#agno-agent) 或许多其他支持 MCP 服务器的 GUI 之一。
-   如果您想在 IDE 中使用 Serena，请参阅[其他 MCP 客户端](#其他-mcp-客户端-cline-roo-code-cursor-windsurf-etc)部分。

Serena 由 `uv` 管理，因此您需要[安装它](https://docs.astral.sh/uv/getting-started/installation/)。

### 运行 Serena MCP 服务器

您有多种选项来运行 MCP 服务器，如下面的小节中所示。

#### 用法

典型用法涉及客户端（Claude Code、Claude Desktop 等）将 MCP 服务器作为子进程运行（使用 stdio 通信），因此客户端需要提供运行 MCP 服务器的命令。
（或者，您可以在 SSE 模式下运行 MCP 服务器，并告诉您的客户端如何连接到它。）

请注意，无论您如何运行 MCP 服务器，Serena 默认都会在 localhost 上启动一个小型 Web 仪表板，该仪表板将显示日志并允许关闭 MCP 服务器（因为许多客户端无法正确清理进程）。
此设置和其他设置可以在[配置](#配置)中和/或通过提供[命令行参数](#命令行参数)进行调整。

###### 本地安装

1.  克隆存储库并进入其中。
    ```shell
    git clone https://github.com/oraios/serena
    cd serena
    ```
2.  （可选）在您的主目录中创建配置文件，即

    *   在 Linux 和 macOS 上为 `~/.serena/serena_config.yml`，或
    *   在 Windows 上为 `%USERPROFILE%\.serena\serena_config.yml`。

    通过复制模板，然后根据您的需要进行调整：
    ```shell
    mkdir ~/.serena
    cp src/serena/resources/serena_config.template.yml ~/.serena/serena_config.yml
    ```
    如果您只想要默认配置，可以跳过此部分，Serena 首次运行时将创建一个配置文件。
3.  使用 `uv` 运行服务器：
    ```shell
    uv run serena-mcp-server
    ```
    从 serena 安装目录外部运行时，请务必传递它，即使用
    ```shell
    uv run --directory /abs/path/to/serena serena-mcp-server
    ```

##### 使用 uvx

`uvx` 可用于直接从存储库运行最新版本的 Serena，无需显式本地安装。

*   Windows:
    ```shell
    uvx --from git+https://github.com/oraios/serena serena-mcp-server.exe
    ```
*   其他操作系统:
    ```shell
    uvx --from git+https://github.com/oraios/serena serena-mcp-server
    ```

##### 使用 Docker（实验性）

⚠️ Docker 支持目前处于实验阶段，并存在一些限制。在使用之前，请阅读 [Docker 文档](DOCKER.md)以了解重要的注意事项。

您可以直接通过 docker 运行 Serena MCP 服务器，如下所示，假设您要处理的所有项目都位于 `/path/to/your/projects` 中：

```shell
docker run --rm -i --network host -v /path/to/your/projects:/workspaces/projects ghcr.io/oraios/serena:latest serena-mcp-server --transport stdio
```

将 `/path/to/your/projects` 替换为您的项目目录的绝对路径。Docker 方法提供：
-   更安全的 shell 命令执行
-   无需在本地安装语言服务器和依赖项
-   跨不同系统的一致环境

有关详细的设置说明、配置选项和已知限制，请参阅 [Docker 文档](DOCKER.md)。

#### SSE 模式

ℹ️ 请注意，使用 stdio 作为协议的 MCP 服务器在客户端/服务器架构方面有些不寻常，因为服务器必须由客户端启动才能通过服务器的标准输入/输出流进行通信。
换句话说，您无需自己启动服务器。客户端应用程序（例如 Claude Desktop）负责此操作，因此需要使用启动命令进行配置。

当使用 SSE 模式（它使用基于 HTTP 的通信）时，您自己控制服务器生命周期，即您启动服务器并向客户端提供连接到它的 URL。

只需为 `serena-mcp-server` 提供 `--transport sse` 选项，并可选地提供端口。
例如，要在端口 9121 上以 SSE 模式运行 Serena MCP 服务器（使用本地安装），您将从 Serena 目录运行此命令，

```shell
uv run serena-mcp-server --transport sse --port 9121
```

然后将您的客户端配置为连接到 `http://localhost/sse:9121`。


#### 命令行参数

Serena MCP 服务器支持广泛的附加命令行选项，包括在 SSE 模式下运行以及使 Serena 适应各种[上下文和操作模式](#模式和上下文)的选项。

运行参数 `--help` 以获取可用选项列表。


### 配置

Serena 的行为（活动工具和提示以及日志配置等）在四个位置进行配置：

1.  `serena_config.yml` 用于适用于所有客户端和项目的通用设置。
    它位于您的用户目录下的 `.serena/serena_config.yml` 中。
    如果您没有明确创建该文件，它将在您首次运行 Serena 时自动生成。
2.  在您的客户端配置中传递给 `serena-mcp-server` 的参数中（见下文），这将适用于由相应客户端启动的所有会话。特别是，[上下文](#上下文)参数应适当设置，以便 Serena 最好地适应您现有客户端的工具和功能。
    有关详细说明，请参阅。您可以通过命令行参数覆盖 `serena_config.yml` 中的所有条目。
3.  您项目中的 `.serena/project.yml` 文件。这将包含项目级配置，只要该项目被激活就会使用该配置。
4.  通过当前激活的[模式](#模式)集。


> ⚠️ **注意：** Serena 正在积极开发中。我们不断添加功能，提高稳定性和用户体验。
> 因此，配置可能会发生破坏性更改。如果您的配置无效，MCP 服务器或基于 Serena 的代理可能会启动失败（在前一种情况下调查 MCP 日志）。
> 更新 Serena 时，请查看[更新日志](CHANGELOG.md)和配置模板，并相应地调整您的配置。

初始设置完成后，根据您使用 Serena 的方式，继续下面的某个部分。

您可以直接要求 LLM 显示您会话的配置，Serena 有一个工具可以做到这一点。

### 项目激活和索引

建议的方法是直接要求 LLM 激活一个项目，提供其绝对路径，或者在项目过去已激活的情况下，提供其名称。默认项目名称是目录名称。

*   “激活项目 /path/to/my_project”
*   “激活项目 my_project”

所有已激活的项目都将自动添加到您的 `serena_config.yml` 中，并且对于每个项目，都将生成 `.serena/project.yml` 文件。您可以调整后者，例如通过更改名称（您在激活时引用的名称）或其他选项。确保没有两个不同项目具有相同的名称。

如果您主要使用同一个项目，您还可以配置在启动时始终激活一个项目，方法是向客户端的 MCP 配置中的 `serena-mcp-server` 命令传递 `--project <path_or_name>`。

ℹ️ 对于大型项目，我们建议您索引项目以加速 Serena 的工具；否则第一次工具应用程序可能会非常慢。
为此，请在项目目录中运行以下命令之一，或将项目路径作为参数传递：

*   使用本地安装时：
    ```shell
    uv run --directory /abs/path/to/serena index-project
    ```
*   使用 uvx 时：
    ```shell
    uvx --from git+https://github.com/oraios/serena index-project
    ```

### Claude Code

Serena 是让 Claude Code 更便宜、更强大的好方法！

在您的项目目录中，使用以下命令添加 serena：

```shell
claude mcp add serena -- <serena-mcp-server> --context ide-assistant --project $(pwd)
```

其中 `<serena-mcp-server>` 是您[运行 Serena MCP 服务器](#运行-serena-mcp-服务器)的方式。
例如，使用 `uvx` 时，您将运行：
```shell
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project $(pwd)
```

ℹ️ Serena 附带一份说明文本，Claude 需要阅读它才能正确使用 Serena 的工具。
一旦进入 Claude Code，您可以要求“阅读 Serena 的初始说明”或运行 `/mcp__serena__initial_instructions` 来加载说明文本。
每当您开始新的对话或在任何压缩操作后，请执行此操作，以确保 Claude 保持正确配置以使用 Serena 的工具。

ℹ️ **新**：上述方法的一种替代方法是将说明作为系统提示的一部分添加，这样您就不需要运行上述命令或记住在压缩后重新运行它。
这可以通过启动 Claude Code 并带有 `claude --append-system-prompt $(uvx --from git+https://github.oraios/serena print-system-prompt)` 来实现。请注意，这是**实验性**功能，Claude 可能无法以这种方式正确理解说明，我们尚未彻底测试由此产生的行为。请报告您遇到的任何问题。


### Claude Desktop

对于 [Claude Desktop](https://claude.ai/download)（适用于 Windows 和 macOS），请转到文件 / 设置 / 开发者 / MCP 服务器 / 编辑配置，
这将允许您打开 JSON 文件 `claude_desktop_config.json`。
添加 `serena` MCP 服务器配置，使用[运行命令](#运行-serena-mcp-服务器)，具体取决于您的设置。

*   本地安装：
    ```json
    {
        "mcpServers": {
            "serena": {
                "command": "/abs/path/to/uv",
                "args": ["run", "--directory", "/abs/path/to/serena", "serena-mcp-server"]
            }
        }
    }
    ```
*   uvx：
    ```json
    {
        "mcpServers": {
            "serena": {
                "command": "/abs/path/to/uvx",
                "args": ["--from", "git+https://github.com/oraios/serena", "serena-mcp-server"]
            }
        }
    }
    ```
*   docker：
    ```json
    {
        "mcpServers": {
            "serena": {
                "command": "docker",
                "args": ["run", "--rm", "-i", "--network", "host", "-v", "/path/to/your/projects:/workspaces/projects", "ghcr.io/oraios/serena:latest", "serena-mcp-server", "--transport", "stdio"]
            }
        }
    }
    ```

如果您在 Windows 上使用包含反斜杠的路径（请注意，您也可以直接使用正斜杠），请务必正确转义它们（`\\`）。

就是这样！保存配置并重新启动 Claude Desktop。您就可以激活您的第一个项目了。

ℹ️ 您可以使用附加参数进一步自定义运行命令（参见[上文](#命令行参数)）。

注意：在 Windows 和 macOS 上有 Anthropic 官方的 Claude Desktop 应用程序，对于 Linux，有一个[开源社区版本](https://github.com/aaddrick/claude-desktop-debian)。

⚠️ 请务必完全退出 Claude Desktop 应用程序，因为关闭 Claude 只会将其最小化到系统托盘——至少在 Windows 上是这样。

⚠️ 某些客户端（目前包括 Claude Desktop）可能会留下僵尸进程。您将不得不手动查找并终止它们。
    使用 Serena，您可以激活[仪表板](#serena-的日志仪表板和-gui-工具)以防止未被注意的进程，并使用仪表板关闭 Serena。

重启后，您应该在聊天界面中看到 Serena 的工具（注意小锤子图标）。

有关 Claude Desktop 与 MCP 服务器的更多信息，请参阅[官方快速入门指南](https://modelcontextprotocol.io/quickstart/user)。

### 其他 MCP 客户端 (Cline, Roo-Code, Cursor, Windsurf, etc.)

作为 MCP 服务器，Serena 可以包含在任何 MCP 客户端中。上述相同的配置，
可能稍作客户端特定修改，应该可以正常工作。大多数流行的
现有编码助手（IDE 扩展或类 VSCode IDE）都支持连接到 MCP 服务器。
**建议**在这些集成中使用 `ide-assistant` 上下文，方法是将 `"--context", "ide-assistant"` 添加到 MCP 客户端配置中的 `args` 中。包含 Serena 通常会通过为它们提供符号操作工具来提高其性能。

在这种情况下，使用费用的计费仍由您选择的客户端控制
（与 Claude Desktop 客户端不同）。但是，您可能仍希望通过这种方法使用 Serena，
例如，出于以下原因之一：

1.  您已经在使用编码助手（例如 Cline 或 Cursor），并且只想使其更强大。
2.  您使用的是 Linux，并且不想使用[社区创建的 Claude Desktop](https://github.com/aaddrick/claude-desktop-debian)。
3.  您希望 Serena 更紧密地集成到您的 IDE 中，并且不介意为此付费。

### Agno 代理

Agno 是一个模型无关的代理框架，它允许您将 Serena 转换为一个代理
（独立于 MCP 技术），支持大量底层 LLM。Agno 是目前
在聊天 GUI 中使用您选择的 LLM 运行 Serena 的最简单方法。通过 Agno，Serena 变成了一个代理
（不再是 MCP 服务器），因此可以以编程方式使用（例如用于基准测试或在您的应用程序中）。

以下是它的工作原理（另请参阅 [Agno 的文档](https://docs.agno.com/introduction/playground)）：

1.  使用 npx 下载 agent-ui 代码
    ```shell
    npx create-agent-ui@latest
    ```
    或者，手动克隆：
    ```shell
    git clone https://github.com/agno-agi/agent-ui.git
    cd agent-ui 
    pnpm install 
    pnpm dev
    ```

2.  安装 serena 并带有可选要求：
    ```shell
    # 您也可以只选择 agno,google 或 agno,anthropic 而不是 all-extras
    uv pip install --all-extras -r pyproject.toml -e .
    ```
    
3.  将 `.env.example` 复制到 `.env` 并填写您打算使用的提供商的 API 密钥。

4.  使用以下命令启动 agno 代理应用程序：
    ```shell
    uv run python scripts/agno_agent.py
    ```
    默认情况下，该脚本使用 Claude 作为模型，但您可以选择 Agno 支持的任何模型
    （这基本上是任何现有模型）。

5.  在新终端中，使用以下命令启动 agno UI：
    ```shell
    cd agent-ui 
    pnpm dev
    ```
    将 UI 连接到您上面启动的代理并开始聊天。您将拥有与 MCP 服务器版本相同的工具。


这是 Serena 使用最新 Gemini 模型执行小型分析任务的简短演示：

https://github.com/user-attachments/assets/ccfcb968-277d-4ca9-af7f-b84578858c62


⚠️ 重要提示：与 MCP 服务器方法不同，Agno UI 中的工具执行不要求用户许可。
Shell 工具尤其关键，因为它可以执行任意代码。虽然我们
在与 Claude 的测试中从未遇到过任何问题，但允许这样做可能并非完全安全。
您可以选择在 Serena 项目的配置文件 (`.yml`) 中禁用某些命令。

### 其他代理框架

将 Serena 集成到任何代理框架（如 [pydantic-ai](https://ai.pydantic.dev/)、[langgraph](https://langchain-ai.github.io/langgraph/tutorials/introduction/) 或其他）中应该很简单。
通常，您只需为 Serena 的工具编写一个适配器，以适应您选择的框架中的工具表示，就像我们为 Agno 编写 [SerenaAgnoToolkit](/src/serena/agno.py) 所做的那样。


## 详细用法和建议

### 工具执行

Serena 将语义代码检索工具与编辑功能和 shell 执行相结合。
Serena 的行为可以通过[模式和上下文](#模式和上下文)进一步自定义。
在[下方](#工具完整列表)找到完整的工具列表。

通常建议使用所有工具，因为这可以使 Serena 提供最大的价值：
只有通过执行 shell 命令（特别是测试），Serena 才能自主识别和纠正错误。

#### Shell 执行和编辑工具

但是，应该注意的是，`execute_shell_command` 工具允许任意代码执行。
当 Serena 作为 MCP 服务器使用时，客户端通常会在执行工具之前请求用户许可，因此只要用户事先检查执行参数，
这就不应该是一个问题。
但是，如果您有疑虑，可以选择在项目的 `.yml` 配置文件中禁用某些命令。
如果您只想纯粹使用 Serena 进行代码分析和建议实现，而无需修改代码库，您可以将项目配置文件中的 `read_only` 设置为 `true`，以启用只读模式。
这将自动禁用所有编辑工具并防止对代码库进行任何修改，同时仍然允许所有分析和探索功能。

总的来说，请务必备份您的工作并使用版本控制系统，以避免丢失任何工作。


### 模式和上下文

Serena 的行为和工具集可以使用上下文和模式进行调整。
这些允许高度自定义，以最好地适应您的工作流程和 Serena 运行的环境。

#### 上下文

上下文定义了 Serena 运行的通用环境。
它影响初始系统提示和可用工具集。
上下文在启动 Serena 时设置（例如，通过 MCP 服务器的 CLI 选项或在代理脚本中），并且在活动会话期间无法更改。

Serena 提供了预定义的上下文：
*   `desktop-app`：专为与 Claude Desktop 等桌面应用程序配合使用而设计。这是默认设置。
*   `agent`：专为 Serena 作为更自主的代理场景而设计，例如与 Agno 一起使用时。
*   `ide-assistant`：优化了与 VSCode、Cursor 或 Cline 等 IDE 的集成，侧重于编辑器内编码辅助。
选择最符合您所用集成类型的上下文。

启动 Serena 时，使用 `--context <context-name>` 指定上下文。
请注意，对于指定参数列表的情况（例如 Claude Desktop），您必须向列表中添加两个参数。

#### 模式

模式进一步细化 Serena 针对特定类型任务或交互风格的行为。可以同时激活多个模式，从而组合它们的效果。模式会影响系统提示，并且还可以通过排除某些工具来更改可用工具集。

内置模式示例包括：
*   `planning`：将 Serena 专注于规划和分析任务。
*   `editing`：优化 Serena 以进行直接代码修改任务。
*   `interactive`：适合对话式、来回交互风格。
*   `one-shot`：将 Serena 配置为在单个响应中完成任务，通常与 `planning` 一起用于生成报告或初始计划。
*   `no-onboarding`：如果特定会话不需要，则跳过初始入职流程。
*   `onboarding`：（通常自动触发）专注于项目入职流程。

模式可以在启动时设置（类似于上下文），但也可以在会话期间**动态切换**。您可以指示 LLM 使用 `switch_modes` 工具来激活不同的模式集（例如，“切换到规划和一次性模式”）。

启动 Serena 时，使用 `--mode <mode-name>` 指定模式；可以指定多个模式，例如 `--mode planning --mode no-onboarding`。

:warning: **模式兼容性**：虽然您可以组合模式，但有些模式在语义上可能不兼容（例如，`interactive` 和 `one-shot`）。Serena 目前不会阻止不兼容的组合；用户需要选择合理的模式配置。

#### 自定义

您可以通过两种方式创建自己的上下文和模式，以精确地根据您的需求定制 Serena：
*   **添加到 Serena 的配置目录**：在本地 Serena 存储库中的 `config/contexts/` 或 `config/modes/` 目录中创建新的 `.yml` 文件。这些自定义上下文/模式将自动注册并可按其名称（不带 `.yml` 扩展名的文件名）使用。它们还将出现在可用上下文/模式的列表中。
*   **使用外部 YAML 文件**：启动 Serena 时，您可以提供自定义上下文或模式的 `.yml` 文件的绝对路径。

上下文或模式 YAML 文件通常定义：
*   `name`：（如果使用文件名，则可选）上下文/模式的名称。
*   `prompt`：将合并到 Serena 系统提示中的字符串。
*   `description`：（可选）简短描述。
*   `excluded_tools`：当此上下文/模式处于活动状态时要禁用的工具名称（字符串）列表。

这种自定义允许深度集成和 Serena 对特定项目要求或个人偏好的适应性。


### 入职培训和记忆

默认情况下，Serena 首次为项目启动时将执行**入职流程**。
入职的目标是让 Serena 熟悉项目并存储记忆，以便在未来的交互中借鉴。
如果 LLM 未能完成入职并且没有实际将相应记忆写入磁盘，您可能需要明确要求它这样做。

入职通常会读取项目中的大量内容，从而填满上下文。因此，一旦入职完成，建议切换到另一个对话。
入职后，我们建议您快速查看记忆，并在必要时编辑或添加新的记忆。

**记忆**是存储在项目目录中的 `.serena/memories/` 中的文件，代理可以选择在后续交互中读取它们。
您可以随意读取和调整它们；您也可以手动添加新的记忆。
`.serena/memories/` 目录中的每个文件都是一个记忆文件。
每当 Serena 开始处理项目时，都会提供记忆列表，代理可以决定读取它们。
我们发现记忆可以显著改善 Serena 的用户体验。


### 准备您的项目

#### 构建您的代码库

Serena 使用代码结构来查找、读取和编辑代码。这意味着它将适用于结构良好的代码，但可能对完全非结构化的代码（例如具有巨大、非模块化函数的“上帝类”）表现不佳。
此外，对于非静态类型语言，类型注释非常有益。

#### 从干净状态开始

最好从干净的 git 状态开始代码生成任务。这不仅会让您更容易检查更改，而且模型本身也有机会通过调用 `git diff` 来查看它更改了什么，从而自行纠正或在后续对话中继续工作。

:warning: **重要提示**：由于 Serena 将使用系统原生的换行符写入文件，并且它可能想要查看 git diff，因此在 Windows 上将 `git config core.autocrlf` 设置为 `true` 非常重要。
在 Windows 上将 `git config core.autocrlf` 设置为 `false`，您最终可能会得到巨大的差异，仅仅是因为换行符。在 Windows 上全局启用此 git 设置通常是一个好主意：

```shell
git config --global core.autocrlf true
```

#### 日志记录、Linting 和自动化测试

Serena 可以成功地在**代理循环**中完成任务，它迭代地获取信息、执行操作并反思结果。
但是，Serena 无法使用调试器；它必须依赖程序执行结果、linting 结果和测试结果来评估其操作的正确性。
因此，设计为有意义的可解释输出（例如日志消息）且具有良好测试覆盖率的软件，对于 Serena 来说更容易处理。

我们通常建议从所有 linting 检查和测试都通过的状态开始编辑任务。

### 提示策略

我们发现，对于非平凡的任务，在实际实施之前花一些时间进行概念化和规划通常是个好主意。这有助于取得更好的结果，并增加控制感和保持在循环中的感觉。您可以在一个会话中制定详细的计划，Serena 可能会读取您的许多代码来建立上下文，然后继续在另一个会话中实施（可能在创建合适的记忆之后）。

### 代码编辑中的潜在问题

根据我们的经验，LLM 在计数方面表现不佳，即它们在正确位置插入代码块时存在问题。大多数编辑操作都可以在符号级别执行，从而克服了这个问题。但是，有时行级插入很有用。

Serena 会仔细检查它将编辑的行号和任何代码块，但如果您遇到问题，您可能会发现明确告诉它如何编辑代码很有用。
我们正在努力使 Serena 的编辑功能更加健壮。

### 上下文耗尽

对于长期复杂的任务，或者 Serena 已读取大量内容的任务，您可能会接近上下文令牌的限制。在这种情况下，通常最好在新对话中继续。Serena 有一个专用工具来创建当前进度状态和所有相关信息的摘要，以便继续。您可以请求创建此摘要并将其写入记忆。然后，在新对话中，您可以要求 Serena 读取记忆并继续任务。根据我们的经验，这非常有效。从好的方面看，由于在单个会话中不涉及摘要，Serena 通常不会迷失方向（不像其他一些在后台进行摘要的代理），并且它还被指示偶尔检查它是否在正确的轨道上。

此外，Serena 被指示在上下文方面要节俭
（例如，不必要地不读取代码符号的主体），
但我们发现 Claude 在节俭方面并不总是很好（Gemini 似乎更好）。
如果您知道不需要，可以明确指示它不要读取主体。

### 将 Serena 与其他 MCP 服务器结合使用

通过 MCP 客户端使用 Serena 时，您可以将其与其他 MCP 服务器一起使用。
但是，请注意工具名称冲突！请参阅上面有关此信息。

目前，与流行的 Filesystem MCP Server 存在冲突。由于 Serena 也提供文件系统操作，因此可能不需要同时启用这两个功能。

### Serena 的日志：仪表板和 GUI 工具

Serena 提供了两种方便的方式来访问当前会话的日志：

*   通过**基于 Web 的仪表板**（默认启用）

    这在所有平台上都受支持。
    默认情况下，它将可在 `http://localhost:24282/dashboard/index.html` 访问，但如果默认端口不可用/有多个实例正在运行，则可能会使用更高的端口。

*   通过 **GUI 工具**（默认禁用）

    这主要在 Windows 上受支持，但也可能在 Linux 上工作；macOS 不受支持。

两者都可以在 Serena 的配置文件 (`serena_config.yml`，见上文) 中启用、配置或禁用。
如果启用，它们将在 Serena 代理/MCP 服务器启动后自动打开。
如果您在配置中将 `record_tool_usage_stats` 设置为 `True`，Web 仪表板将显示 Serena 工具的使用统计信息。

除了查看日志之外，这两个工具还允许关闭 Serena 代理。
提供此功能是因为 Claude Desktop 等客户端在自身关闭时可能无法终止 MCP 服务器子进程。

### 故障排除

Claude Desktop 中对 MCP 服务器的支持以及各种 MCP 服务器 SDK 都是相对较新的开发，可能会出现不稳定情况。

MCP 服务器的有效配置可能因平台和客户端而异。我们建议始终使用绝对路径，因为相对路径可能会导致错误。
语言服务器在单独的子进程中运行，并使用 asyncio 调用——有时客户端可能会使其崩溃。如果您启用了 Serena 的日志窗口，并且它消失了，您就会知道发生了什么。

某些客户端可能无法正确终止 MCP 服务器，请注意挂起的 python 进程并在必要时手动终止它们。

## 与其他编码代理的比较

据我们所知，Serena 是第一个功能齐全的编码代理，其所有功能都通过 MCP 服务器提供，因此不需要 API 密钥或订阅。

### 基于订阅的编码代理

最著名的基于订阅的编码代理是 IDE 的一部分，例如 Windsurf、Cursor 和 VSCode。
Serena 的功能类似于 Cursor 的 Agent、Windsurf 的 Cascade 或 VSCode 即将推出的[代理模式](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode)。

Serena 的优势在于无需订阅。
一个潜在的缺点是它没有直接集成到 IDE 中，因此新编写代码的检查不那么无缝。

更多技术差异包括：
*   Serena 不受特定 IDE 的限制。
    Serena 的 MCP 服务器可以与任何 MCP 客户端（包括某些 IDE）一起使用，并且基于 Agno 的代理提供了应用其功能的其他方式。
*   Serena 不受特定大型语言模型或 API 的限制。
*   Serena 使用语言服务器导航和编辑代码，因此它对代码有符号理解。
    基于 IDE 的工具通常使用基于 RAG 或纯文本的方法，这种方法通常功能较弱，尤其是在大型代码库中。
*   Serena 是开源的，代码库很小，因此可以轻松扩展和修改。

### 基于 API 的编码代理

基于订阅的代理的替代方案是基于 API 的代理，例如 Claude Code、Cline、Aider、Roo Code 等，其中使用成本直接映射到底层 LLM 的 API 成本。
其中一些（例如 Cline）甚至可以作为扩展包含在 IDE 中。
它们通常非常强大，其主要缺点是（可能非常高）API 成本。

Serena 本身可以用作基于 API 的代理（参见上面关于 Agno 的部分）。
我们尚未为 Serena 编写 CLI 工具或专用 IDE 扩展（而且可能也不需要后者，因为 Serena 已经可以与任何支持 MCP 服务器的 IDE 一起使用）。
如果需要 Serena 作为类似 Claude Code 的 CLI 工具，我们将考虑编写一个。

Serena 与其他基于 API 的代理之间的主要区别在于，Serena 还可以用作 MCP 服务器，因此不需要 API 密钥并绕过 API 成本。这是 Serena 的独特功能。

### 其他基于 MCP 的编码代理

还有其他用于编码的 MCP 服务器，例如 [DesktopCommander](https://github.com/wonderwhy-er/DesktopCommanderMCP) 和 [codemcp](https://github.com/ezyang/codemcp)。
然而，据我们所知，它们都没有提供语义代码检索和编辑工具；它们纯粹依赖于基于文本的分析。
正是语言服务器和 MCP 的集成使 Serena 独一无二，并使其在具有挑战性的编码任务中（尤其是在大型代码库的上下文中）如此强大。


## 致谢

Serena 建立在多种现有开源技术之上，其中最重要的是：

1.  [multilspy](https://github.com/microsoft/multilspy)。
    一个包装语言服务器实现并使其适应通过 Python 交互的库，它为我们的 Solid-LSP 库（src/solidlsp）提供了基础。
    Solid-LSP 提供纯同步 LSP 调用，并使用 Serena 所需的符号逻辑扩展了原始库。
2.  [Python MCP SDK](https://github.com/modelcontextprotocol/python-sdk)
3.  [Agno](https://github.com/agno-agi/agno) 和
    相关的 [agent-ui](https://github.com/agno-agi/agent-ui)，
    我们使用它们来允许 Serena 与任何模型一起工作，超越了
    支持 MCP 的模型。
4.  我们通过 Solid-LSP 使用的所有语言服务器。

没有这些项目，Serena 就不可能实现（或者会更难构建）。


## 定制和扩展 Serena

用您自己的想法扩展 Serena 的 AI 功能非常简单。
只需通过子类化 `serena.agent.Tool` 并实现与工具要求匹配的签名的 `apply` 方法即可实现一个新工具。
一旦实现，`SerenaAgent` 将自动访问新工具。

添加对新编程语言的支持也相对简单（参阅[/CONTRIBUTING.md#adding-a-new-supported-language](/CONTRIBUTING.md#adding-a-new-supported-language)）。

我们期待看到社区将创造出什么！
有关贡献的详细信息，请参阅[此处](/CONTRIBUTING.md)。

## 工具完整列表

以下是 Serena 工具的完整列表及其简短说明（`uv run serena-list-tools` 的输出）：

*   `activate_project`：按名称激活项目。
*   `check_onboarding_performed`：检查是否已执行项目入职培训。
*   `create_text_file`：在项目目录中创建/覆盖文件。
*   `delete_lines`：删除文件中的一系列行。
*   `delete_memory`：从 Serena 的项目特定记忆存储中删除记忆。
*   `execute_shell_command`：执行 shell 命令。
*   `find_referencing_code_snippets`：查找引用给定位置符号的代码片段。
*   `find_referencing_symbols`：查找引用给定位置符号的符号（可选地按类型过滤）。
*   `find_symbol`：对具有/包含给定名称/子字符串的符号执行全局（或本地）搜索（可选地按类型过滤）。
*   `get_active_project`：获取当前活动项目的名称（如果有）并列出现有项目。
*   `get_current_config`：打印代理的当前配置，包括活动模式、工具和上下文。
*   `get_symbols_overview`：获取给定文件或目录中定义的顶级符号的概述。
*   `initial_instructions`：获取当前项目的初始说明。
    仅应在无法设置系统提示的环境中使用，例如在您无法控制的客户端（如 Claude Desktop）中。
*   `insert_after_symbol`：在给定符号定义的末尾插入内容。
*   `insert_at_line`：在文件中的给定行插入内容。
*   `insert_before_symbol`：在给定符号定义的开头插入内容。
*   `list_dir`：列出给定目录中的文件和目录（可选地递归）。
*   `list_memories`：列出 Serena 的项目特定记忆存储中的记忆。
*   `onboarding`：执行入职培训（识别项目结构和基本任务，例如测试或构建）。
*   `prepare_for_new_conversation`：提供准备新对话的说明（以便在必要上下文中继续）。
*   `read_file`：读取项目目录中的文件。
*   `read_memory`：从 Serena 的项目特定记忆存储中读取具有给定名称的记忆。
*   `replace_lines`：用新内容替换文件中的一系列行。
*   `replace_symbol_body`：替换符号的完整定义。
*   `restart_language_server`：重新启动语言服务器，当发生非 Serena 的编辑时可能需要。
*   `search_for_pattern`：在项目中搜索模式。
*   `summarize_changes`：提供总结对代码库所做更改的说明。
*   `switch_modes`：通过提供模式名称列表来激活模式。
*   `think_about_collected_information`：用于思考所收集信息完整性的思维工具。
*   `think_about_task_adherence`：用于确定代理是否仍在当前任务轨道上的思维工具。
*   `think_about_whether_you_are_done`：用于确定任务是否真正完成的思维工具。
*   `write_memory`：将命名记忆（供将来参考）写入 Serena 的项目特定记忆存储。 