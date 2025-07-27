# 🚀 Serena 快速参考卡片

## 📋 常用命令速查

### 🔧 安装和启动
```bash
# 安装uv包管理器
curl -LsSf https://astral.sh/uv/install.sh | sh  # macOS/Linux
# 或 powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"  # Windows

# 克隆项目
git clone https://github.com/oraios/serena.git && cd serena

# 安装依赖
uv pip install --all-extras -r pyproject.toml -e .

# 启动MCP服务器
uv run serena-mcp-server

# 启动并指定项目
uv run serena-mcp-server --project $(pwd)

# 调试模式启动
uv run serena-mcp-server --log-level DEBUG --trace-lsp-communication
```

### 🎛️ 常用启动参数
```bash
--project /path/to/project     # 指定项目路径
--context ide-assistant        # 设置上下文
--mode planning               # 设置模式
--transport sse               # 使用SSE传输
--port 9121                   # 指定端口
--tool-timeout 300            # 工具超时时间
--enable-web-dashboard        # 启用Web仪表板
```

### 📁 配置文件位置
```bash
~/.serena/serena_config.yml           # 全局配置
{project}/.serena/project.yml         # 项目配置
src/serena/resources/*.template.yml   # 配置模板
```

---

## 💬 AI对话常用指令

### 🎯 项目管理
```
激活项目 /path/to/project
激活项目 my_project
获取当前配置
列出所有项目
```

### 🔍 代码分析
```
查找符号 "ClassName"
获取 src/main.py 的符号概览
查找引用 src/main.py 第10行第5列的符号
搜索模式 "def.*function"
```

### 📝 文件操作
```
读取文件 src/main.py
创建文件 new_file.py 内容：[代码内容]
在第5行后插入：[新代码]
替换第10-15行为：[新代码]
删除第20-25行
```

### ⚡ 命令执行
```
执行命令：python main.py
执行命令：git status
执行命令：pytest tests/
```

### 🧠 记忆管理
```
写入记忆 "memory_name" 内容：[记忆内容]
读取记忆 "memory_name"
列出所有记忆
删除记忆 "memory_name"
```

### 🔄 系统控制
```
重启语言服务器
切换到规划模式
切换到编辑模式
检查入门指导是否完成
```

---

## 🌐 Web仪表板功能

### 📊 访问地址
- **默认地址**：http://localhost:24282/dashboard/
- **备用端口**：24283, 24284, 24285...

### 🎛️ 主要功能
- ✅ **实时日志查看** - 监控所有系统活动
- ✅ **工具使用统计** - 查看工具调用频率和性能
- ✅ **服务器控制** - 远程关闭服务器
- ✅ **配置查看** - 查看当前配置状态

### 📡 API端点
```bash
GET  /dashboard/                    # 仪表板首页
POST /get_log_messages             # 获取日志消息
GET  /get_tool_names               # 获取工具列表
GET  /get_tool_stats               # 获取工具统计
POST /clear_tool_stats             # 清除统计数据
PUT  /shutdown                     # 关闭服务器
```

---

## 🔧 支持的编程语言

| 语言 | 配置值 | 语言服务器 | 安装要求 |
|------|--------|------------|----------|
| Python | `python` | Pyright/Jedi | ✅ 内置 |
| TypeScript | `typescript` | TSServer | ✅ 内置 |
| Java | `java` | Eclipse JDTLS | JDK |
| C# | `csharp` | OmniSharp | .NET + .sln |
| Rust | `rust` | rust-analyzer | Rust工具链 |
| Go | `go` | Gopls | Go + gopls |
| C/C++ | `cpp` | clangd | clang |
| PHP | `php` | Intelephense | PHP |
| Ruby | `ruby` | Solargraph | Ruby + gem |
| Elixir | `elixir` | NextLS | Elixir |
| Clojure | `clojure` | clojure-lsp | Clojure |

---

## 🛠️ 40+ 工具快速索引

### 📁 文件操作工具
- `read_file` - 读取文件内容
- `create_text_file` - 创建/覆盖文件
- `list_dir` - 列出目录内容
- `search_for_pattern` - 搜索模式

### 🔍 符号分析工具
- `find_symbol` - 查找符号
- `find_referencing_symbols` - 查找引用符号
- `find_referencing_code_snippets` - 查找引用代码片段
- `get_symbols_overview` - 获取符号概览

### ✏️ 代码编辑工具
- `insert_at_line` - 在指定行插入
- `insert_before_symbol` - 在符号前插入
- `insert_after_symbol` - 在符号后插入
- `replace_lines` - 替换行范围
- `replace_symbol_body` - 替换符号体
- `delete_lines` - 删除行范围

### ⚡ 执行工具
- `execute_shell_command` - 执行Shell命令

### 🧠 记忆工具
- `write_memory` - 写入记忆
- `read_memory` - 读取记忆
- `list_memories` - 列出记忆
- `delete_memory` - 删除记忆

### ⚙️ 配置工具
- `activate_project` - 激活项目
- `get_current_config` - 获取当前配置
- `switch_modes` - 切换模式
- `restart_language_server` - 重启语言服务器

### 🎯 工作流工具
- `onboarding` - 执行入门指导
- `check_onboarding_performed` - 检查入门状态
- `prepare_for_new_conversation` - 准备新对话
- `summarize_changes` - 总结变更

### 🤔 思考工具
- `think_about_collected_information` - 思考收集的信息
- `think_about_task_adherence` - 思考任务执行情况
- `think_about_whether_you_are_done` - 思考是否完成

---

## 🚨 故障排除速查

### ❌ 常见错误及解决方案

| 错误类型 | 症状 | 解决方案 |
|----------|------|----------|
| **Python版本错误** | 安装失败 | 确保使用Python 3.11 |
| **端口冲突** | 仪表板无法访问 | 使用 `--port` 指定其他端口 |
| **语言服务器超时** | 符号查找失败 | 执行 `重启语言服务器` |
| **权限错误** | 文件操作失败 | 检查文件权限和路径 |
| **工具超时** | 工具执行卡住 | 使用 `--tool-timeout` 增加时间 |
| **配置文件错误** | 启动失败 | 检查YAML语法和路径 |

### 🔍 调试技巧
```bash
# 启用详细日志
uv run serena-mcp-server --log-level DEBUG

# 跟踪LSP通信
uv run serena-mcp-server --trace-lsp-communication

# 查看进程状态
ps aux | grep serena

# 检查端口占用
netstat -tulpn | grep 24282
```

---

## 🎯 最佳实践提示

### ✅ 推荐做法
- 🎯 **项目激活**：每次开始工作前先激活正确的项目
- 📝 **记忆管理**：为重要信息创建记忆，便于后续引用
- 🔍 **符号查找**：优先使用符号级操作而非文本搜索
- 📊 **监控日志**：定期查看Web仪表板了解系统状态
- 🔄 **定期重启**：长时间使用后重启语言服务器

### ❌ 避免做法
- ❌ 不要在单次对话中读取过多文件
- ❌ 不要忽略错误信息和警告
- ❌ 不要在没有备份的情况下进行大量编辑
- ❌ 不要同时运行多个Serena实例
- ❌ 不要在生产环境中使用调试模式

---

## 📞 获取帮助

### 🔗 官方资源
- **GitHub仓库**：https://github.com/oraios/serena
- **问题报告**：GitHub Issues
- **文档**：项目README和分析文档

### 📚 本地文档
- [完整教程](tutorial.md)
- [架构分析](architecture.md)
- [配置详解](configuration.md)
- [API接口](api_interfaces.md)

### 💡 快速诊断
```bash
# 检查系统状态
uv run serena-mcp-server --help

# 查看工具列表
uv run python scripts/print_tool_overview.py

# 测试工具运行
uv run python scripts/demo_run_tools.py
```

---

**🎯 记住：这张参考卡片是您的Serena使用指南，建议收藏备用！**
