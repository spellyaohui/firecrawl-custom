<h3 align="center">
  <a name="readme-top"></a>
  <img
    src="https://raw.githubusercontent.com/firecrawl/firecrawl/main/img/firecrawl_logo.png"
    height="200"
  >
</h3>

<div align="center">

# Firecrawl 自定义版本

**支持豆包等第三方 LLM API 的修改版本**

</div>

---

## 重要声明

> **本项目是 [Firecrawl](https://github.com/firecrawl/firecrawl) 的修改版本（Fork）**
>
> - **原项目**：https://github.com/firecrawl/firecrawl
> - **原作者**：Sideguide Technologies Inc. / Firecrawl 团队
> - **原项目许可证**：AGPL-3.0
>
> 本修改版本遵循原项目的 AGPL-3.0 开源协议。根据该协议要求：
> - 本项目的所有修改内容均以相同协议开源
> - 如果您部署本项目的修改版本作为网络服务，必须向用户提供完整源代码
> - 本项目保留原项目的所有版权声明和许可证信息
>
> **免责声明**：本项目仅供学习和研究使用。使用者应自行承担使用本软件的风险，并遵守相关法律法规。

---

## 修改内容

本版本在原版 Firecrawl 基础上进行了以下修改：

- **支持第三方 LLM API**：支持豆包（Doubao）等兼容 OpenAI API 格式的第三方大语言模型服务
- **自定义 API 端点**：可配置自定义的 LLM API 基础 URL

---

## 什么是 Firecrawl？

[Firecrawl](https://firecrawl.dev?ref=github) 是一个 API 服务，可以抓取 URL 并将其转换为干净的 Markdown 或结构化数据。它能够爬取所有可访问的子页面，并为每个页面提供干净的数据，无需站点地图。查看[官方文档](https://docs.firecrawl.dev)了解更多。

## 如何使用？

官方提供了托管版本的 API，您可以在[这里](https://firecrawl.dev/playground)找到演示和文档。您也可以自行部署后端。

### 相关资源

- **API**：[文档](https://docs.firecrawl.dev/api-reference/introduction)
- **SDK**：[Python](https://docs.firecrawl.dev/sdks/python)、[Node](https://docs.firecrawl.dev/sdks/node)
- **LLM 框架**：[Langchain (Python)](https://python.langchain.com/docs/integrations/document_loaders/firecrawl/)、[Langchain (JS)](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/firecrawl)、[Llama Index](https://docs.llamaindex.ai/en/latest/examples/data_connectors/WebPageDemo/#using-firecrawl-reader)、[Crew.ai](https://docs.crewai.com/)
- **低代码框架**：[Dify](https://dify.ai/blog/dify-ai-blog-integrated-with-firecrawl)、[Langflow](https://docs.langflow.org/)、[Flowise AI](https://docs.flowiseai.com/integrations/langchain/document-loaders/firecrawl)
- **社区 SDK**：[Go](https://docs.firecrawl.dev/sdks/go)、[Rust](https://docs.firecrawl.dev/sdks/rust)

本地运行请参考[贡献指南](https://github.com/firecrawl/firecrawl/blob/main/CONTRIBUTING.md)。

### 功能特性

- **抓取（Scrape）**：抓取 URL 并获取 LLM 可用格式的内容（Markdown、结构化数据、截图、HTML）
- **爬取（Crawl）**：抓取网页的所有 URL 并返回 LLM 可用格式的内容
- **映射（Map）**：输入网站获取所有网站 URL - 速度极快
- **搜索（Search）**：搜索网络并获取结果的完整内容
- **提取（Extract）**：使用 AI 从单页、多页或整个网站获取结构化数据

### 强大能力

- **LLM 可用格式**：Markdown、结构化数据、截图、HTML、链接、元数据
- **处理复杂场景**：代理、反爬机制、动态内容（JS 渲染）、输出解析、编排
- **高度可定制**：排除标签、通过自定义请求头爬取需要认证的页面、最大爬取深度等
- **媒体解析**：PDF、DOCX、图片
- **可靠性优先**：专为获取您需要的数据而设计 - 无论多困难
- **操作（Actions）**：在提取数据前执行点击、滚动、输入、等待等操作
- **批量处理**：使用新的异步端点同时抓取数千个 URL
- **变更追踪**：监控和检测网站内容随时间的变化

您可以在[文档](https://docs.firecrawl.dev)中找到 Firecrawl 的所有功能及使用方法。

---

## API 使用示例

### 爬取网站

用于爬取 URL 及其所有可访问的子页面。这会提交一个爬取任务并返回任务 ID 以检查爬取状态。

```bash
curl -X POST https://api.firecrawl.dev/v2/crawl \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer fc-YOUR_API_KEY' \
    -d '{
      "url": "https://docs.firecrawl.dev",
      "limit": 10,
      "scrapeOptions": {
        "formats": ["markdown""]
      }
    }'
```

返回爬取任务 ID 和检查状态的 URL：

```json
{
  "success": true,
  "id": "123-456-789",
  "url": "https://api.firecrawl.dev/v2/crawl/123-456-789"
}
```

### 检查爬取任务

用于检查爬取任务的状态并获取结果：

```bash
curl -X GET https://api.firecrawl.dev/v2/crawl/123-456-789 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY'
```

### 抓取单页

用于抓取 URL 并以指定格式获取其内容：

```bash
curl -X POST https://api.firecrawl.dev/v2/scrape \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "url": "https://docs.firecrawl.dev",
      "formats" : ["markdown", "ht }'
```

### 网站映射

用于映射 URL 并获取网站的所有 URL：

```bash
curl -X POST https://api.firecrawl.dev/v2/map \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "url": "https://firecrawl.dev"
    }'
```

### 搜索

搜索网络并获取结果的完整内容：

```bash
curl -X POST https://api.firecrawl.dev/v2/search \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer fc-YOUR_API_KEY" \
  -d '{
    "query": "what is firecrawl?",
    "limit": 5
  }'
```

### 数据提取（Beta）

使用提示词和/或 Schema 从整个网站获取结构化数据：

```bash
curl -X POST https://api.firecrawl.dev/v2/extract \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "urls": ["https://firecrawl.dev/*"],
      "prompt": "从页面中提取公司使命、是否开源、是否在 Y Combinator。",
      "schema": {
        "type": "object",
        "properties": {
          "company_mission": { "type": "string" },
          "is_open_source": { "type": "boolean" },
          "is_in_yc": { "type": "boolean" }
        }
      }
    }'
```

---

## 使用 Python SDK

### 安装

```bash
pip install firecrawl-py
```

### 爬取网站

```python
from firecrawl import Firecrawl

firecrawl = Firecrawl(api_key="fc-YOUR_API_KEY")

# 抓取网站（返回 Document）
doc = firecrawl.scrape(
    "https://firecrawl.dev",
    formats=["markdown", "html"],
)
print(doc.markdown)

# 爬取网站
response = firecrawl.crawl(
    "https://firecrawl.dev",
    limit=100,
    scrape_options={"formats": ["markdown", "html"]},
    poll_interval=30,
)
print(response)
```

### 提取结构化数据

```python
from pydantic import BaseModel, Field
from typing import List

class Article(BaseModel):
    title: str
    points: int
    by: str
    commentsURL: str

class TopArticles(BaseModel):
    top: List[Article] = Field(..., description="前 5 篇文章")

# 使用 JSON 格式和 Pydantic Schema
doc = firecrawl.scrape(
    "https://news.ycombinator.com",
    formats=[{"type": "json", "schema": TopArticles}],
)
print(doc.json)
```

---

## 使用 Node SDK

### 安装

```bash
npm install @mendable/firecrawl-js
```

### 使用方法

```js
import Firecrawl from '@mendable/firecrawl-js';

const firecrawl = new Firecrawl({ apiKey: 'fc-YOUR_API_KEY' });

// 抓取网站
const doc = await firecrawl.scrape('https://firecrawl.dev', {
  formats: ['markdown', 'html'],
});
console.log(doc);

// 爬取网站
const response = await firecrawl.crawl('https://firecrawl.dev', {
  limit: 100,
  scrapeOptions: { formats: ['markdown', 'html'] },
});
console.log(response);
```

---

## 开源版与云服务

Firecrawl 以 AGPL-3.0 许可证开源。

官方同时提供托管版本的云服务，详见 [firecrawl.dev](https://firecrawl.dev)。

![开源版与云服务对比](https://raw.githubusercontent.com/firecrawl/firecrawl/main/img/open-source-cloud.png)

---

## 贡献

欢迎贡献！请在提交 Pull Request 前阅读[贡献指南](CONTRIBUTING.md)。如需自托管，请参考[自托管指南](SELF_HOST.md)。

---

## 使用条款

**使用 Firecrawl 进行抓取、搜索和爬取时，最终用户有责任遵守目标网站的政策。建议用户在开始任何抓取活动前，遵守目标网站的隐私政策和使用条款。默认情况下，Firecrawl 在爬取时会遵守网站 robots.txt 文件中指定的指令。使用 Firecrawl 即表示您明确同意遵守这些条件。**

---

## 许可证声明

本项目主要采用 GNU Affero 通用公共许可证 v3.0（AGPL-3.0）授权，详见仓库根目录的 LICENSE 文件。但某些组件采用 MIT 许可证授权。

请注意：
- AGPL-3.0 许可证适用于项目的所有部分，除非另有说明
- SDK 和部分 UI 组件采用 MIT 许可证授权，详见相应目录中的 LICENSE 文件
- 使用或贡献本项目时，请确保遵守您所使用的特定组件的相应许可证条款

---

## 致谢

感谢 [Firecrawl](https://github.com/firecrawl/firecrawl) 原项目及其贡献者。

<a href="https://github.com/firecrawl/firecrawl/graphs/contributors">
  <img alt="contributors" src="https://contrib.rocks/image?repo=firecrawl/firecrawl"/>
</a>

---

<p align="right" style="font-size: 14px; color: #555; margin-top: 20px;">
    <a hreadme-top" style="text-decoration: none; color: #007bff; font-weight: bold;">
        ↑ 返回顶部 ↑
    </a>
</p>
