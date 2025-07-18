# 经验总结

本文档简要收集了我们在开发和使用 Serena 过程中所学到的经验，包括哪些方面做得好，哪些方面不尽如人意。

## 成功之处

### 将工具逻辑与 MCP 实现分离

MCP 只是另一种协议，不应让其细节渗透到应用程序逻辑中。官方文档建议使用函数注解来定义工具和提示。虽然这对于小型项目可以快速启动，但对于更严肃的项目来说并不明智。在 Serena 中，所有工具都独立定义，然后使用我们的 `make_tool` 函数转换为 `MCPTool` 实例。

### 自动生成的 PromptFactory

提示模板对于大多数 LLM 应用程序至关重要，因此需要在代码中对其进行良好的表示，同时它们通常需要可定制并暴露给用户。在 Serena 中，我们通过在单独的 YAML 文件中定义提示模板（jinja 格式，用户可以轻松修改）并从这些 YAML 文件中自动生成具有有意义的方法和参数名称的 `PromptFactory` 类来解决这些相互冲突的需求。后者已提交到我们的代码中。我们将生成逻辑分离到 [interprompt](/src/interprompt/README.md) 子包中，该子包可以用作库。

### 用于编辑工具测试的临时文件和快照

我们通过在 `tests/resources` 中为每种支持的语言设置一个小型“项目”来测试 Serena 的大多数方面。对于会更改这些项目中代码的编辑工具，我们使用临时文件来复制代码。非常棒的 [syrupy](https://github.com/syrupy-project/syrupy) pytest 插件有助于开发快照测试。

### 用于日志记录的仪表板和 GUI

了解 MCP 服务器正在做什么非常有用。我们收集并在 GUI 或 Web 仪表板中显示日志，这对于查看正在发生的事情和识别任何问题非常有帮助。

### 不受限制的 Bash 工具

我们知道允许在沙箱外执行无限的 shell 命令并非特别安全，但我们进行了一些评估，到目前为止……还没有发生任何坏事。看来当前版本的 AI 主宰者很少会执行 `sudo rm -rf /`。尽管如此，我们仍在研究一种更安全的方法以及与沙箱的更好集成。

### Multilspy

[multilspy](https://github.com/microsoft/multilspy/) 项目在我们的入门阶段提供了很大帮助，并且是 Serena 的核心。许多更知名的 Python 语言服务器实现的代码质量和设计都 subpar（例如，缺少类型）。

### 用 Serena 开发 Serena

我们清楚地注意到，工具越好，就越容易使其变得更好。

## 提示

### 可能需要大声喊叫和情感语言

在开发 `ReplaceRegexTool` 时，我们最初无法让 Claude 4（在 Claude Desktop 中）使用通配符来节省输出令牌。无论是示例还是明确的指令都没有帮助。只有在初始指令和工具描述中添加了

```
IMPORTANT: REMEMBER TO USE WILDCARDS WHEN APPROPRIATE! I WILL BE VERY UNHAPPY IF YOU WRITE LONG REGEXES WITHOUT USING WILDCARDS INSTEAD!
```

之后，Claude 才最终开始遵循指令。

## 失败之处

### MCP 客户端的生命周期处理

MCP 技术显然非常不成熟。尽管 MCP SDK 中有生命周期上下文，但包括 Claude Desktop 在内的许多客户端都未能正确清理，留下了僵尸进程。我们通过 GUI 窗口和仪表板来缓解这个问题，以便用户可以看到 Serena 是否正在运行并可以在那里终止它。

### 信任 Asyncio

运行多个 asyncio 应用程序导致非确定性事件循环污染和死锁，这非常难以调试和理解。我们用一个大锤解决了这个问题，将所有 asyncio 应用程序放入一个单独的进程中。这使得代码更加复杂，并略微增加了 RAM 需求，但这似乎是可靠地克服 asyncio 死锁问题的唯一方法。

### 跨操作系统 Tkinter GUI

不同的操作系统在启动窗口或处理 Tkinter 安装方面有不同的限制。这太麻烦了，我们转而使用 Web 仪表板。

### 基于行号的编辑

LLM 不仅在计数方面出了名的差，而且编辑操作后行号也会改变，LLM 也常常太笨而无法理解它们应该更新之前收到的行号信息。我们转向了基于字符串匹配和符号名称的编辑。 