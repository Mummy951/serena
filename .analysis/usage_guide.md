# 🚀 Serena 使用指南

## 📋 环境要求

### 系统要求
- **操作系统**：Windows、macOS、Linux
- **Python版本**：Python 3.11 (严格要求，不支持3.12+)
- **内存**：建议4GB以上
- **磁盘空间**：至少1GB可用空间

### 必需工具
- **uv** - Python包管理器和虚拟环境工具
- **Git** - 版本控制系统
- **语言服务器** - 根据项目语言自动安装

## 🔧 安装部署

### 方法一：本地安装 (推荐)

#### 1. 安装uv包管理器

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

#### 2. 克隆项目
```bash
git clone https://github.com/oraios/serena
cd serena
```

#### 3. 创建配置文件
```bash
# 创建全局配置目录
mkdir ~/.serena

# 复制配置模板
cp src/serena/resources/serena_config.template.yml ~/.serena/serena_config.yml
```

#### 4. 安装依赖
```bash
# 基础安装
uv pip install -r pyproject.toml -e .

# 完整安装(包含所有可选功能)
uv pip install --all-extras -r pyproject.toml -e .
```

### 方法二：使用uvx (无需本地安装)

**Windows:**
```bash
uvx --from git+https://github.com/oraios/serena serena-mcp-server.exe
```

**其他系统:**
```bash
uvx --from git+https://github.com/oraios/serena serena-mcp-server
```

### 方法三：Docker部署 (实验性)

#### 使用Docker Compose (推荐)

**生产模式:**
```bash
docker-compose up serena
```

**开发模式:**
```bash
docker-compose up serena-dev
```

#### 直接使用Docker
```bash
# 构建镜像
docker build -t serena .

# 运行容器
docker run -it --rm \
  -v "$(pwd)":/workspace \
  -p 9121:9121 \
  -p 24282:24282 \
  -e SERENA_DOCKER=1 \
  serena
```

## 🎯 运行方式

### 1. MCP服务器模式 (主要用法)

#### 基础启动
```bash
uv run serena-mcp-server
```

#### 带参数启动
```bash
# 指定项目和上下文
uv run serena-mcp-server --project /path/to/project --context ide-assistant

# 多模式启动
uv run serena-mcp-server --mode planning --mode interactive

# SSE模式启动
uv run serena-mcp-server --transport sse --port 9121

# 调试模式
uv run serena-mcp-server --log-level DEBUG --trace-lsp-communication
```

#### 客户端集成配置

**Claude Desktop配置 (claude_desktop_config.json):**
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

**Claude Code配置:**
```bash
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project $(pwd)
```

### 2. Agno代理模式

#### 配置API密钥
```bash
# 复制环境变量模板
cp .env.example .env

# 编辑.env文件，添加API密钥
# ANTHROPIC_API_KEY=your_key_here
# OPENAI_API_KEY=your_key_here
# GOOGLE_API_KEY=your_key_here
```

#### 启动Agno代理
```bash
# 安装Agno相关依赖
uv pip install --all-extras -r pyproject.toml -e .

# 启动代理服务
uv run python scripts/agno_agent.py

# 在新终端启动UI
npx create-agent-ui@latest
cd agent-ui
pnpm install
pnpm dev
```

## 🛠️ 命令行工具

### 核心命令

| 命令 | 功能 | 用法示例 |
|------|------|----------|
| `serena-mcp-server` | 启动MCP服务器 | `uv run serena-mcp-server --project /path/to/project` |
| `index-project` | 项目索引 | `uv run index-project /path/to/project` |
| `print-system-prompt` | 打印系统提示 | `uv run print-system-prompt` |

### MCP服务器参数详解

| 参数 | 类型 | 默认值 | 描述 | 示例 |
|------|------|--------|------|------|
| `--project` | str | None | 项目路径或名称 | `--project /home/user/myproject` |
| `--context` | str | `desktop-app` | 运行上下文 | `--context ide-assistant` |
| `--mode` | str | `[]` | 运行模式(可多个) | `--mode planning --mode interactive` |
| `--transport` | str | `stdio` | 传输协议 | `--transport sse` |
| `--host` | str | `0.0.0.0` | 监听地址 | `--host localhost` |
| `--port` | int | `8000` | 监听端口 | `--port 9121` |
| `--enable-web-dashboard` | bool | None | 启用Web仪表板 | `--enable-web-dashboard` |
| `--enable-gui-log-window` | bool | None | 启用GUI日志窗口 | `--enable-gui-log-window` |
| `--log-level` | str | None | 日志级别 | `--log-level DEBUG` |
| `--trace-lsp-communication` | bool | None | 跟踪LSP通信 | `--trace-lsp-communication` |
| `--tool-timeout` | float | None | 工具超时时间 | `--tool-timeout 300` |

### 项目索引工具

```bash
# 索引当前目录项目
uv run index-project

# 索引指定项目
uv run index-project /path/to/project

# 带日志级别的索引
uv run index-project --log-level DEBUG /path/to/project
```

### 开发工具命令

```bash
# 代码格式化
uv run poe format

# 代码检查
uv run poe lint

# 类型检查
uv run poe type-check

# 运行测试
uv run poe test

# 打印工具概览
uv run python scripts/print_tool_overview.py

# 演示工具运行
uv run python scripts/demo_run_tools.py
```

## 🔧 项目激活和配置

### 项目激活方式

#### 方法一：通过LLM激活
```
"激活项目 /path/to/my_project"
"激活项目 my_project"  # 如果已注册
```

#### 方法二：启动时激活
```bash
uv run serena-mcp-server --project /path/to/project
```

#### 方法三：配置文件激活
在`~/.serena/serena_config.yml`中添加：
```yaml
projects:
  - /path/to/project1
  - /path/to/project2
```

### 项目配置文件

每个项目会自动生成`.serena/project.yml`配置文件：

```yaml
# 项目基本信息
project_name: "my_project"
language: python  # 支持的语言见下表
encoding: utf-8

# 文件忽略配置
ignore_all_files_in_gitignore: true
ignored_paths:
  - "*.log"
  - "temp/**"
  - "__pycache__/**"

# 访问控制
read_only: false  # 只读模式

# 工具配置
excluded_tools: []  # 排除的工具
included_optional_tools: []  # 包含的可选工具

# 项目特定提示
initial_prompt: ""
```

### 支持的编程语言

| 语言 | 配置值 | 语言服务器 | 安装要求 |
|------|--------|------------|----------|
| Python | `python` | Pyright/Jedi | 无 |
| TypeScript/JavaScript | `typescript` | TSServer | 无 |
| Java | `java` | Eclipse JDTLS | JDK |
| C# | `csharp` | OmniSharp | .NET SDK + .sln文件 |
| Rust | `rust` | rust-analyzer | Rust工具链 |
| Go | `go` | Gopls | Go + gopls |
| C/C++ | `cpp` | clangd | clang |
| PHP | `php` | Intelephense | PHP |
| Ruby | `ruby` | Solargraph | Ruby + gem |
| Elixir | `elixir` | NextLS | Elixir (Windows不支持) |
| Clojure | `clojure` | clojure-lsp | Clojure |

## 🌐 Web仪表板

### 访问方式
- **默认地址**：`http://localhost:24282/dashboard/index.html`
- **端口冲突**：自动使用24283、24284等端口
- **Docker模式**：可通过环境变量`SERENA_DASHBOARD_PORT`配置

### 功能特性
- ✅ 实时日志查看
- ✅ 工具使用统计
- ✅ 服务器状态监控
- ✅ 远程关闭服务器
- ✅ 配置信息查看

## 🐛 故障排除

### 常见问题

#### 1. 语言服务器启动失败
```bash
# 检查语言服务器状态
uv run serena-mcp-server --trace-lsp-communication

# 重启语言服务器
# 在LLM中执行：restart_language_server
```

#### 2. 端口冲突
```bash
# 使用不同端口
uv run serena-mcp-server --port 9121

# SSE模式
uv run serena-mcp-server --transport sse --port 9121
```

#### 3. 权限问题
```bash
# 确保uv在PATH中
which uv

# 检查文件权限
ls -la ~/.serena/
```

#### 4. Docker相关问题
```bash
# 检查Docker状态
docker ps

# 查看容器日志
docker logs <container_id>

# 重新构建镜像
docker build --no-cache -t serena .
```

### 日志调试

#### 启用详细日志
```bash
uv run serena-mcp-server --log-level DEBUG --trace-lsp-communication
```

#### 查看日志文件
- **Web仪表板**：`http://localhost:24282/dashboard/`
- **GUI窗口**：启用`--enable-gui-log-window`
- **终端输出**：直接在启动终端查看

### 性能优化

#### 项目索引
```bash
# 大型项目建议预先索引
uv run index-project /path/to/large/project
```

#### 内存优化
```bash
# 调整工具超时时间
uv run serena-mcp-server --tool-timeout 120

# 启用只读模式(项目配置)
read_only: true
```

---

*更多详细信息请参考项目README.md和相关文档*
