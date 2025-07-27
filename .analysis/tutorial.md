# 🎓 Serena 使用教程 - 从零到精通

## 📚 教程概览

本教程将带您从零开始，一步步学会使用Serena这个强大的编码代理工具包。无论您是初学者还是有经验的开发者，都能通过本教程快速上手。

### 🎯 学习目标
- ✅ 理解Serena的核心概念和工作原理
- ✅ 掌握Serena的安装和配置
- ✅ 学会使用Serena的主要功能
- ✅ 能够独立解决常见问题
- ✅ 了解高级用法和最佳实践

### ⏱️ 预计学习时间
- **快速入门**：30分钟
- **基础使用**：1小时
- **进阶功能**：2小时
- **完整掌握**：4小时

---

## 🚀 第一步：环境准备

### 1.1 系统要求检查

首先确认您的系统满足以下要求：

```bash
# 检查Python版本（必须是3.11）
python --version
# 应该显示：Python 3.11.x

# 检查Git是否安装
git --version
# 应该显示：git version x.x.x
```

**❌ 如果Python版本不对**：
- Windows: 从 [python.org](https://python.org) 下载Python 3.11
- macOS: `brew install python@3.11`
- Linux: `sudo apt install python3.11` 或使用对应包管理器

### 1.2 安装uv包管理器

uv是Serena使用的现代Python包管理器，比pip更快更可靠。

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**验证安装:**
```bash
uv --version
# 应该显示版本号
```

### 1.3 选择您的使用场景

Serena支持多种使用方式，请根据您的需求选择：

| 使用场景 | 推荐方式 | 适用人群 |
|----------|----------|----------|
| 🎯 **Claude用户** | MCP服务器 + Claude Desktop | 想要免费使用Claude的用户 |
| 🤖 **多模型用户** | Agno代理模式 | 想要使用GPT、Gemini等多种模型 |
| 💻 **IDE集成** | MCP服务器 + VSCode/Cursor | 在IDE中使用编程助手 |
| 🔬 **开发测试** | 本地开发模式 | 开发者和研究人员 |

---

## 🎯 第二步：快速入门（推荐新手）

### 2.1 克隆项目

```bash
# 克隆Serena项目
git clone https://github.com/oraios/serena.git
cd serena

# 查看项目结构
ls -la
```

### 2.2 一键安装

```bash
# 安装所有依赖（包含可选功能）
uv pip install --all-extras -r pyproject.toml -e .
```

**💡 安装说明：**
- `--all-extras`: 安装所有可选功能（Agno、Anthropic、Google等）
- `-e .`: 以开发模式安装，方便后续更新

### 2.3 创建配置文件

```bash
# 创建配置目录
mkdir -p ~/.serena

# 复制配置模板
cp src/serena/resources/serena_config.template.yml ~/.serena/serena_config.yml

# 查看配置文件
cat ~/.serena/serena_config.yml
```

### 2.4 第一次运行

```bash
# 启动Serena MCP服务器
uv run serena-mcp-server
```

**🎉 成功标志：**
- 看到 "MCP server lifetime setup complete" 消息
- Web仪表板自动在浏览器中打开（http://localhost:24282/dashboard/）
- 没有错误信息

**❌ 如果出现错误：**
- 检查Python版本是否为3.11
- 确认所有依赖都已正确安装
- 查看错误信息并参考故障排除部分

---

## 🔧 第三步：配置您的第一个项目

### 3.1 准备测试项目

我们使用Serena自身作为测试项目：

```bash
# 确保在serena目录中
pwd
# 应该显示：/path/to/serena

# 查看项目文件
ls src/serena/
```

### 3.2 启动并激活项目

```bash
# 启动Serena并指定项目
uv run serena-mcp-server --project $(pwd)
```

**📝 观察启动过程：**
1. 配置文件加载
2. 项目自动注册
3. 语言服务器初始化（Python - Pyright）
4. Web仪表板启动
5. MCP服务器就绪

### 3.3 验证项目激活

打开Web仪表板（http://localhost:24282/dashboard/），您应该看到：
- ✅ 实时日志显示
- ✅ 项目激活成功的消息
- ✅ 语言服务器启动日志

---

## 🎮 第四步：连接您的第一个客户端

### 4.1 方案A：Claude Desktop（推荐）

#### 下载Claude Desktop
- 访问 [claude.ai/download](https://claude.ai/download)
- 下载适合您系统的版本
- 安装并启动

#### 配置MCP服务器
1. 打开Claude Desktop
2. 点击 **File → Settings → Developer → MCP Servers → Edit Config**
3. 在打开的JSON文件中添加：

```json
{
    "mcpServers": {
        "serena": {
            "command": "/path/to/uv",
            "args": ["run", "--directory", "/path/to/serena", "serena-mcp-server"]
        }
    }
}
```

**🔧 路径配置说明：**
- 将 `/path/to/uv` 替换为您的uv路径（运行 `which uv` 查看）
- 将 `/path/to/serena` 替换为Serena项目的绝对路径

#### 重启并测试
1. 完全退出Claude Desktop（确保从系统托盘也退出）
2. 重新启动Claude Desktop
3. 在聊天中输入：`读取Serena的初始说明`

**✅ 成功标志：**
- 看到工具图标（小锤子）
- Claude能够读取Serena的说明文档
- 可以执行工具调用

### 4.2 方案B：Agno代理模式

#### 配置API密钥
```bash
# 复制环境变量模板
cp .env.example .env

# 编辑.env文件
nano .env  # 或使用您喜欢的编辑器
```

在`.env`文件中添加您的API密钥：
```bash
# 选择您要使用的模型对应的API密钥
ANTHROPIC_API_KEY=your_anthropic_key_here
OPENAI_API_KEY=your_openai_key_here
GOOGLE_API_KEY=your_google_key_here
```

#### 启动Agno代理
```bash
# 启动代理服务
uv run python scripts/agno_agent.py
```

#### 启动Web界面
```bash
# 在新终端窗口中
npx create-agent-ui@latest
cd agent-ui
pnpm install
pnpm dev
```

然后在浏览器中访问显示的地址（通常是 http://localhost:3000）。

---

## 🛠️ 第五步：学习基本操作

### 5.1 项目激活和管理

#### 激活项目
在Claude或Agno界面中输入：
```
激活项目 /path/to/your/project
```

或者使用项目名称（如果已注册）：
```
激活项目 my_project
```

#### 查看项目状态
```
获取当前配置
```

这会显示：
- 当前激活的项目
- 可用的工具列表
- 配置信息

### 5.2 代码分析操作

#### 查找符号
```
在项目中查找名为 "SerenaAgent" 的符号
```

#### 查看文件结构
```
获取 src/serena/agent.py 文件的符号概览
```

#### 读取文件内容
```
读取文件 src/serena/README.md
```

### 5.3 代码编辑操作

#### 创建新文件
```
创建一个新文件 test_example.py，内容如下：
def hello_world():
    print("Hello from Serena!")
```

#### 修改文件
```
在 test_example.py 文件的第2行后插入一行注释：
# This is a test function
```

### 5.4 执行命令

#### 运行Python脚本
```
执行命令：python test_example.py
```

#### 查看Git状态
```
执行命令：git status
```

### 5.5 记忆管理

#### 写入记忆
```
写入一个记忆，名称为 "project_overview"，内容是：
这是Serena项目的测试，主要功能包括代码分析、编辑和执行。
```

#### 读取记忆
```
读取记忆 "project_overview"
```

#### 列出所有记忆
```
列出所有记忆
```

---

## 🎯 第六步：实战练习

### 6.1 练习1：代码分析任务

**目标：** 分析Serena项目的核心架构

**步骤：**
1. 激活Serena项目
2. 查找 `SerenaAgent` 类
3. 分析该类的方法和属性
4. 查看相关的导入和依赖
5. 总结架构设计

**示例对话：**
```
用户：请帮我分析Serena项目的核心架构，从SerenaAgent类开始

AI：我来帮您分析Serena项目的核心架构。首先让我查找SerenaAgent类...
[执行工具调用]
```

### 6.2 练习2：代码修改任务

**目标：** 为项目添加一个简单的工具函数

**步骤：**
1. 创建新的Python文件
2. 实现一个简单的工具函数
3. 添加适当的文档字符串
4. 测试函数功能
5. 提交更改

### 6.3 练习3：项目管理任务

**目标：** 管理多个项目

**步骤：**
1. 创建一个新的测试项目
2. 配置项目的语言和设置
3. 在不同项目间切换
4. 比较不同项目的配置

---

## 🔍 第七步：高级功能

### 7.1 自定义配置

#### 项目级配置
编辑 `.serena/project.yml`：
```yaml
# 自定义项目配置
project_name: "my_custom_project"
language: python
read_only: false
excluded_tools: []
initial_prompt: "这是我的自定义项目提示"
```

#### 全局配置
编辑 `~/.serena/serena_config.yml`：
```yaml
# 启用工具使用统计
record_tool_usage_stats: true

# 调整日志级别
log_level: 10  # DEBUG级别

# 自定义工具超时
tool_timeout: 300
```

### 7.2 模式和上下文

#### 切换到规划模式
```
切换到规划模式和交互模式
```

#### 使用不同上下文
```bash
# 启动时指定IDE助手上下文
uv run serena-mcp-server --context ide-assistant
```

### 7.3 监控和调试

#### 查看实时日志
- 访问 Web仪表板：http://localhost:24282/dashboard/
- 观察工具调用和执行过程
- 查看性能统计

#### 启用调试模式
```bash
# 启用详细日志和LSP通信跟踪
uv run serena-mcp-server --log-level DEBUG --trace-lsp-communication
```

---

## 🚨 第八步：故障排除

### 8.1 常见问题解决

#### 问题1：语言服务器启动失败
**症状：** 符号查找功能不工作
**解决：**
```bash
# 重启语言服务器
# 在AI对话中输入：
重启语言服务器
```

#### 问题2：端口冲突
**症状：** Web仪表板无法访问
**解决：**
```bash
# 使用不同端口
uv run serena-mcp-server --port 9121
```

#### 问题3：工具执行超时
**症状：** 工具调用长时间无响应
**解决：**
```bash
# 增加超时时间
uv run serena-mcp-server --tool-timeout 600
```

### 8.2 日志分析

#### 查看详细日志
1. 打开Web仪表板
2. 查看最近的错误信息
3. 根据错误类型采取对应措施

#### 常见错误模式
- `LanguageServerException`: 语言服务器问题
- `FileNotFoundError`: 文件路径问题
- `TimeoutError`: 超时问题
- `PermissionError`: 权限问题

---

## 🎓 第九步：最佳实践

### 9.1 项目组织建议

#### 目录结构
```
your_project/
├── .serena/
│   ├── project.yml      # 项目配置
│   └── memories/        # 项目记忆
├── src/                 # 源代码
├── tests/              # 测试代码
├── docs/               # 文档
└── README.md           # 项目说明
```

#### 配置管理
- 为每个项目创建独立的配置
- 使用有意义的项目名称
- 定期备份配置文件

### 9.2 工作流程建议

#### 开始新任务
1. 激活相关项目
2. 阅读或创建相关记忆
3. 分析现有代码结构
4. 制定实施计划
5. 逐步实施并测试

#### 代码审查
1. 使用符号查找功能
2. 分析代码依赖关系
3. 检查代码风格一致性
4. 运行测试验证功能

### 9.3 性能优化

#### 大型项目优化
```bash
# 预先索引项目
uv run index-project /path/to/large/project

# 启用只读模式（如果只需要分析）
# 在project.yml中设置：
read_only: true
```

#### 内存管理
- 定期清理不需要的记忆
- 避免在单次对话中读取过多文件
- 使用项目切换来管理上下文

---

## 🎉 恭喜！您已完成Serena教程

### 📚 您现在掌握了：
- ✅ Serena的安装和配置
- ✅ 基本的代码分析和编辑操作
- ✅ 项目管理和切换
- ✅ 高级功能和自定义配置
- ✅ 故障排除和性能优化

### 🚀 下一步建议：
1. **实践项目**：在您的实际项目中使用Serena
2. **探索工具**：尝试所有40+种工具的功能
3. **自定义扩展**：根据需要开发自定义工具
4. **社区参与**：加入Serena社区，分享经验

### 📖 进一步学习资源：
- [官方文档](https://github.com/oraios/serena)
- [架构分析](architecture.md)
- [API接口文档](api_interfaces.md)
- [配置系统详解](configuration.md)

**🎯 记住：** Serena是一个强大的工具，最好的学习方式就是在实际项目中使用它。开始您的编程代理之旅吧！
