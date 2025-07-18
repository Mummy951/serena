# Serena 的 Docker 设置（实验性）

⚠️ **实验性功能**：Serena 的 Docker 设置目前处于实验阶段，并存在一些限制。在使用 Docker 和 Serena 之前，请阅读本文档的全部内容。

## 概述

Docker 支持允许您在隔离的容器环境中运行 Serena，这为 shell 工具提供了更好的安全隔离，并在不同系统之间提供了一致的依赖关系。

## 优势

-   **更安全的 shell 工具执行**：命令在隔离的容器环境中运行
-   **一致的依赖关系**：无需在主机系统上管理语言服务器和依赖关系
-   **跨平台支持**：在 Windows、macOS 和 Linux 上一致运行

## 重要限制和注意事项

### 1. 配置文件冲突

⚠️ **关键**：Docker 使用单独的配置文件 (`serena_config.docker.yml`) 来避免路径冲突。在 Docker 中运行时：
-   容器路径将存储在配置中（例如，`/workspaces/serena/...`）
-   这些路径与非 Docker 用法不兼容
-   使用 Docker 后，您无法在不手动调整配置的情况下直接切换回非 Docker 用法

### 2. 项目激活限制

-   **仅限挂载的目录有效**：项目必须挂载为卷才能访问
-   挂载目录之外的项目无法激活或访问
-   默认设置仅挂载当前目录

### 3. GUI 窗口禁用

-   Docker 环境中会自动禁用 GUI 日志窗口选项
-   请改用 Web 仪表板（见下文）

### 4. 仪表板端口配置

Web 仪表板默认在端口 24282 (0x5EDA) 上运行。您可以使用环境变量配置此项：

```bash
# 使用默认端口
docker-compose up serena

# 使用自定义端口
SERENA_DASHBOARD_PORT=8080 docker-compose up serena
```

⚠️ **注意**：如果本地端口被占用，您需要使用环境变量指定不同的端口。

### 5. Windows 上的换行符问题

⚠️ **Windows 用户**：请注意潜在的换行符不一致：
-   在 Docker 容器中编辑的文件可能使用 Unix 换行符 (LF)
-   您的 Windows 系统可能期望 Windows 换行符 (CRLF)
-   这可能会导致版本控制和文本编辑器出现问题
-   适当地配置您的 Git 设置：`git config core.autocrlf true`

## 快速开始

### 使用 Docker Compose（推荐）

1.  **生产模式**（用于将 Serena 用作 MCP 服务器）：
    ```bash
    docker-compose up serena
    ```

2.  **开发模式**（挂载源代码）：
    ```bash
    docker-compose up serena-dev
    ```

### 直接使用 Docker

```bash
# 构建镜像
docker build -t serena .

# 运行并挂载当前目录
docker run -it --rm \
  -v "$(pwd)":/workspace \
  -p 9121:9121 \
  -p 24282:24282 \
  -e SERENA_DOCKER=1 \
  serena
```

## 访问仪表板

运行后，在以下地址访问 Web 仪表板：
-   默认：http://localhost:24282/dashboard
-   自定义端口：http://localhost:${SERENA_DASHBOARD_PORT}/dashboard

## 卷挂载

要使用项目，您必须将它们挂载为卷：

```yaml
# 在 compose.yaml 中
volumes:
  - ./my-project:/workspace/my-project
  - /path/to/another/project:/workspace/another-project
```

## 环境变量

-   `SERENA_DOCKER=1`：自动设置以指示 Docker 环境
-   `SERENA_PORT`：MCP 服务器端口（默认：9121）
-   `SERENA_DASHBOARD_PORT`：Web 仪表板端口（默认：24282）

## 故障排除

### 端口已被占用

如果您看到“端口已被占用”错误：
```bash
# 检查哪个进程正在使用该端口
lsof -i :24282  # macOS/Linux
netstat -ano | findstr :24282  # Windows

# 使用不同的端口
SERENA_DASHBOARD_PORT=8080 docker-compose up serena
```

### 配置问题

如果您需要重置 Docker 配置：
```bash
# 删除 Docker 特定配置
rm serena_config.docker.yml

# Serena 将在下次运行时自动生成新的配置
```

### 项目访问问题

确保项目已正确挂载：
-   检查 `docker-compose.yaml` 中的卷挂载
-   对外部项目使用绝对路径
-   验证挂载目录的权限

## 迁移路径

要在 Docker 和非 Docker 用法之间切换：

1.  **Docker 到非 Docker**：
    -   手动编辑 `serena_config.yml` 中的项目路径
    -   将容器路径更改为主机路径
    -   或者为每个环境使用单独的配置文件

2.  **非 Docker 到 Docker**：
    -   项目将使用容器路径重新注册
    -   原始配置保持不变

## 未来改进

我们正在努力：
-   环境之间的自动配置迁移
-   更好的项目路径处理
-   动态端口分配
-   Windows 换行符处理。 