gui_log_window: False
# 是否打开一个带有 Serena 日志的图形窗口。
# 这主要在 Windows 和（部分）Linux 上受支持；macOS 上不可用。
# 如果您想在 Web 浏览器中查看日志，请改用 `web_dashboard` 选项。
# 限制：似乎不适用于 Claude Desktop 的 Linux 社区版
# 也可能导致某些 MCP 客户端出现问题 - 如果您遇到任何问题，请尝试禁用此选项

# 能够检查日志对于故障排除和监控工具调用都很有用，
# 尤其是在使用 agno playground 时，因为工具调用不总是显示，
# 并且输入参数从不显示在 agno UI 中。
# 当用作 Claude Desktop 的 MCP 服务器时，日志主要用于故障排除。
# 注意：不幸的是，启动 Serena 服务器或代理的各种实体以神秘的方式进行，
# 通常会启动多个进程实例而不关闭以前的实例。这可能导致打开多个日志窗口，
# 并且只有最后一个窗口会更新。由于我们无法控制 agno 或 Claude Desktop 如何启动 Serena，
# 目前我们不得不接受这个限制。

web_dashboard: True
# 是否打开 Serena Web 仪表板（可通过您的 Web 浏览器访问），它将显示 Serena 当前的会话日志
# - 作为 GUI 日志窗口的替代方案，GUI 日志窗口在所有平台上都受支持。

web_dashboard_open_on_launch: True
# Serena 启动时是否打开带有 Web 仪表板的浏览器窗口（前提是启用了 `web_dashboard`）。
# 如果设置为 False，您仍然可以通过在 Web 浏览器中导航到 http://localhost:24282/dashboard/
# 手动打开仪表板（24282 = 0x5EDA，Serena Dashboard）。
# 如果您有多个实例正在运行，将使用更高的端口；请尝试端口 24283、24284 等。

log_level: 20
# GUI 日志窗口和仪表板的最低日志级别（10 = 调试，20 = 信息，30 = 警告，40 = 错误）

trace_lsp_communication: False
# 是否跟踪 Serena 与语言服务器之间的通信。
# 这对于调试语言服务器问题很有用。

tool_timeout: 240
# 工具执行的超时时间，单位为秒

excluded_tools: []
# 全局排除的工具列表

included_optional_tools: []
# 要包含的可选工具列表（默认禁用）

jetbrains: False
# 是否启用 JetBrains 模式并使用基于 Serena JetBrains IDE 插件的工具
# 而不是基于语言服务器的工具
# 注意：该插件尚未发布。这仅适用于 Serena 开发人员。


record_tool_usage_stats:  False
# 是否记录工具使用统计信息，如果记录处于活动状态，它们将显示在 Web 仪表板中。

token_count_estimator: TIKTOKEN_GPT4O
# 仅当 `record_tool_usage` 为 True 时相关；用于工具使用统计的令牌计数估计器的名称。
# 有关可用选项，请参阅 `RegisteredTokenCountEstimator` 枚举。
#
# 注意：某些令牌估计器（如 tiktoken）可能在首次运行时需要下载数据文件，
# 这可能需要一些时间并需要互联网连接。其他，如 Anthropic 的，可能需要 API 密钥
# 并可能适用速率限制。


# 由 SERENA 管理，请保持在 YAML 文件的底部，除非必要，否则不要编辑
# 注册项目列表。
# 要添加项目，在聊天中，只需让 Serena “激活项目 /path/to/project” 或者，
# 如果项目以前已添加，则“激活项目 <项目名称>”。
# 默认情况下，项目的名称将是包含该项目的目录的名称，但您可以通过编辑
# （自动生成的）项目配置文件 `/path/project/project/.serena/project.yml` 文件来更改它。
# 如果您想完全控制项目配置，请手动创建 project.yml 文件，然后
# 指示 Serena 通过其路径首次激活项目。
# 注意：确保注册项目的名称没有名称冲突。
projects: [] 