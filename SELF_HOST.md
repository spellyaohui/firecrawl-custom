# 自托管 Firecrawl

#### 贡献者？

欢迎来到 [Firecrawl](https://firecrawl.dev) 🔥！以下是如何在本地运行项目的说明，以便您可以自行运行并参与贡献。

如果您想贡献代码，流程与其他开源项目类似：Fork Firecrawl、进行修改、运行测试、提交 PR。

如有任何问题或需要帮助，请加入我们的 Discord 社区 [这里](https://discord.gg/gSmWdAkdwd) 获取更多信息，或在 Github 上提交 Issue [这里](https://github.com/firecrawl/firecrawl/issues/new/choose)！

## 为什么要自托管？

自托管 Firecrawl 对于有严格安全策略、要求数据保留在受控环境中的组织特别有益。以下是考虑自托管的一些关键原因：

- **增强的安全性和合规性：** 通过自托管，您可以确保所有数据处理符合内部和外部法规，将敏感信息保留在您的安全基础设施内。请注意，Firecrawl 是 Mendable 的产品，依赖于 SOC2 Type2 认证，这意味着该平台遵循高行业标准来管理数据安全。
- **可定制的服务：** 自托管允许您定制服务（如 Playwright 服务），以满足特定需求或处理标准云服务可能不支持的特定用例。
- **学习和社区贡献：** 通过设置和维护您自己的实例，您可以更深入地了解 Firecrawl 的工作原理，这也可能带来对项目更有意义的贡献。

### 注意事项

但是，有一些限制和额外的责任需要注意：

1. **Fire-engine 访问受限：** 目前，自托管的 Firecrawl 实例无法访问 Fire-engine，其中包括处理 IP 封锁、机器人检测机制等高级功能。这意味着虽然您可以管理基本的抓取任务，但更复杂的场景可能需要额外配置或可能不受支持。
2. **需要手动配置：** 如果您需要使用基本 fetch 和 Playwright 选项之外的抓取方法，您需要在 `.env` 文件中手动配置这些。这需要对技术有更深入的了解，可能涉及更多的设置时间。

自托管 Firecrawl 非常适合需要完全控制其抓取和数据处理环境的用户，但需要权衡额外的维护和配置工作。

## 步骤

1. 首先，安装依赖

- Docker [安装说明](https://docs.docker.com/get-docker/)


2. 设置环境变量

使用以下模板在根目录创建 `.env` 文件。

`.env` 文件内容：
```
# ===== 必需的环境变量 ======
PORT=3002
HOST=0.0.0.0

# 注意：PORT 同时用于主 API 服务器和 Worker 存活检查端点

# 要启用数据库身份验证，需要设置 Supabase
USE_DB_AUTHENTICATION=false

# ===== 可选的环境变量 ======

## === AI 功能（抓取时的 JSON 格式、/extract API）===
# 在此处提供您的 OpenAI API 密钥以启用 AI 功能
# OPENAI_API_KEY=

# 实验性：使用 Ollama
# OLLAMA_BASE_URL=http://localhost:11434/api
# MODEL_NAME=deepseek-r1:7b
# MODEL_EMBEDDING_NAME=nomic-embed-text

# 实验性：使用任何兼容 OpenAI 的 API
# OPENAI_BASE_URL=https://example.com/v1
# OPENAI_API_KEY=

## === 代理 ===
# PROXY_SERVER 可以是完整 URL（例如 http://0.1.2.3:1234）或仅 IP 和端口组合（例如 0.1.2.3:1234）
# 如果您的代理不需要身份验证，请不要取消注释 PROXY_USERNAME 和 PROXY_PASSWORD
# PROXY_SERVER=
# PROXY_USERNAME=
# PROXY_PASSWORD=

## === /search API ===
# 默认情况下，/search API 将使用 Google 搜索。

# 如果您想使用 SearXNG 而不是直接使用 Google，可以指定启用了 JSON 格式的 SearXNG 服务器。
# 您还可以自定义 engines 和 categories 参数，但默认值也应该可以正常工作。
# SEARXNG_ENDPOINT=http://your.searxng.server
# SEARXNG_ENGINES=
# SEARXNG_CATEGORIES=

## === 其他 ===

# Supabase 设置（用于支持数据库身份验证、高级日志记录等）
# SUPABASE_ANON_TOKEN=
# SUPABASE_URL=
# SUPABASE_SERVICE_TOKEN=

# 如果已设置身份验证并想使用真实 API 密钥进行测试，请使用此项
# TEST_API_KEY=

# 此密钥允许您访问队列管理面板。如果您的部署可公开访问，请更改此项。
BULL_AUTH_KEY=CHANGEME

# 这现在由 docker-compose.yaml 自动配置。您不需要设置它。
# PLAYWRIGHT_MICROSERVICE_URL=http://playwright-service:3000/scrape
# REDIS_URL=redis://redis:6379
# REDIS_RATE_LIMIT_URL=redis://redis:6379
tgreSQL 数据库配置 ===
# 配置 PostgreSQL 凭据。这些应与 nuq-postgres 容器使用的凭据匹配。
# 如果更改这些，请确保所有三个设置一致。
# POSTGRES_USER=firecrawl
# POSTGRES_PASSWORD=firecrawl_password
# POSTGRES_DB=firecrawl

# 如果您有 LlamaParse 密钥用于解析 PDF，请设置此项
# LLAMAPARSE_API_KEY=

# 如果您想将服务器健康状态消息发送到 Slack，请设置此项
# SLACK_WEBHOOK_URL=

## === 系统资源配置 ===
# 最大 CPU 使用阈值（0.0-1.0）。当 CPU 使用率超过此值时，Worker 将拒绝新任务。
# 默认值：0.8（80%）
# MAX_CPU=0.8

# 最大 RAM 使用阈值（0.0-1.0）。当内存使用率超过此值时，Worker 将拒绝新任务。
# 默认值：0.8（80%）
# MAX_RAM=0.8

# 如果您想允许将本地 Webhook 发送到您的自托管实例，请设置此项
# ALLOW_LOCAL_WEBHOOKS=true
```

### 安全注意事项

- **使用强 PostgreSQL 凭据`.env` 模板中的默认值仅用于本地开发。部署到服务器时，请将 `POSTGRES_USER`、`POSTGRES_PASSWORD` 和 `POSTGRES_DB` 设置为安全值，并确保它们与数据库服务配置匹配。
- **保持数据库端口内部化。** 提供的 `docker-compose.yaml` 不会将 PostgreSQL 暴露给主机或互联网。除非您使用防火墙限制访问，否则避免为 `nuq-postgres` 添加 `ports` 映射。要访问数据库进行维护，建议使用 `docker compose exec nuq-postgres psql` 或临时的、有防火墙保护的隧道。
- **保护管理 UI。** 将 `BULL_AUTH_KEY` 设置为强密钥，特别是在任何可从不受信任网络访问的部署上。

3. 构建并运行 Docker 容器：

    ```bash
    docker compose build
    docker compose up
    ```

    如果遇到错误，请确保使用的是 `docker compose` 而不是 `docker-compose`。

    这将运行一个本地 Firecrawl 实例，可以通过 `http://localhost:3002` 访问。

    您应该能够在 `http://localhost:3002/admin/CHANGEME/queues` 看到 Bull 队列管理器 UI。

5. *（可选）* 测试 API

如果您想测试爬取端点，可以运行：

  ```bash
  curl -X POST http://localhost:3002/v1/crawl \
      -H 'Content-Type: application/json' \
      -d '{
        "url": "https://firecrawl.dev"
      }'
  ```

## 故障排除

本节提供您在设置或运行自托管 Firecrawl 实例时可能遇到的常见问题的解决方案。

### SDK 使用的 API 密钥

**注意：** 在自托管实例中使用 Firecrawl SDK 时，API 密钥是可选的。API 密钥仅在连接到云服务 (api.firecrawl.dev) 时才需要。

### Supabase 客户端未配置

**症状：**
```bash
[YYYY-MM-DDTHH:MM:SS.SSSz]ERROR - Attempted to access Supabase client when it's not configured.
[YYYY-MM-DDTHH:MM:SS.SSSz]ERROR - Error inserting scrape event: Error: Supabase client is not configured.
```

**说明：**
此错误是因为 Supabase 客户端设置未完成。您应该能够正常进行抓取和爬取。目前无法在自托管实例中配置 Supabase。

### 您正在绕过身份验证

**症状：**
```bash
[YYYY-MM-DDTHH:MM:SS.SSSz]WARN - You're bypassing authentication
```

**说明：**
此错误是因为 Supabase 客户端设置未完成。您应该能够正常进行抓取和爬取。目前无法在自托管实例中配置 Supabase。

### Docker 容器无法启动

**症状：**
Docker 容器意外退出或无法启动。

**解决方案：**
使用以下命令检查 Docker 日志中的任何错误消息：
```bash
docker logs [container_name]
```

- 确保 `.env` 文件中正确设置了所有必需的环境变量。
- 验证 `docker-compose.yml` 中定义的所有 Docker 服务是否正确配置，以及必要的镜像是否可用。

### Redis 连接问题

**症状：**
与 Redis 连接相关的错误，如超时或"连接被拒绝"。

**解决方案：**
- 确保 Redis 服务在您的 Docker 环境中正在运行。
- 验证 `.env` 文件中的 `REDIS_URL` 和 `REDIS_RATE_LIMIT_URL` 指向正确的 Redis 实例，确保它指向 `docker-compose.yaml` 文件中的相同 URL（`redis://redis:6379`）。
- 检查可能阻止连接到 Redis 端口的网络设置和防火墙规则。

### API 端点无响应

**症状：**
对 Firecrawl 实例的 API 请求超时或无响应。

**解决方案：**
- 通过检查 Docker 容器状态确保 Firecrawl 服务正在运行。
- 验证 `.env` 文件中的 `PORT` 和 `HOST` 设置是否正确，以及没有其他服务使用相同的端口。
- 检查网络配置以确保主机可从发出 API 请求的客户端访问。

通过解决这些常见问题，您可以确保自托管 Firecrawl 实例的设置和运行更加顺畅。

## 在 Kubernetes 集群上安装 Firecrawl（简单版本）

阅读 [examples/kubernetes/cluster-install/README.md](https://github.com/firecrawl/firecrawl/blob/main/examples/kubernetes/cluster-install/README.md) 了解如何在 Kubernetes 集群上安装 Firecrawl 的说明。

## 使用 Helm 在 Kubernetes 集群上安装 Firecrawl

阅读 [examples/kubernetes/firecrawl-helm/README.md](https://github.com/firecrawl/firecrawl/blob/main/examples/kubernetes/firecrawl-helm/README.md) 了解如何使用 Helm 在 Kubernetes 集群上安装 Firecrawl 的说明。
