# 服务器搜索工具配置经验

在服务器上配置了三套搜索工具：**AnySearch**（MCP 搜索服务，主查询引擎）、**Tavily**（MCP 搜索服务）和 **Firecrawl**（CLI 抓取/搜索工具）。互补使用。

---

## 0. 总览

AnySearch 是目前的主搜索引擎，会话中会通过 MCP 工具直接使用。Tavily 和 Firecrawl 是备选和补充。

---

## 1. AnySearch MCP（主搜索引擎）

AnySearch 是目前 Hermes 配置的主搜索引擎，作为 MCP 服务器集成。

### 配置

在 `~/.hermes/config.yaml` 的 `mcp_servers` 中：

```yaml
mcp_servers:
  anysearch:
    url: https://api.anysearch.com/mcp
    headers:
      Authorization: Bearer as_sk_xxx...
    timeout: 30
```

### 提供的工具

| 工具 | 功能 |
|------|------|
| `mcp_anysearch_search` | 主搜索（支持垂直领域路由：finance/academic/business/health/legal 等+通用搜索） |
| `mcp_anysearch_batch_search` | 批量搜索（最多5条并行） |
| `mcp_anysearch_get_sub_domains` | 获取垂直搜索子领域 |
| `mcp_anysearch_extract` | 提取网页完整内容 |

### 使用模式

- 通用查询直接调用 `search`
- 多意图/多领域查询用 `batch_search` 并行发送
- 垂直搜索需要先 `get_sub_domains` 获取子领域，再搜索
- 查询结果不够时用 `extract` 获取全文

---

## 2. Tavily MCP（备选搜索）

### 安装

```bash
npm install -g tavily-mcp
```

### 配置

在 `~/.hermes/config.yaml` 的 `mcp_servers` 中添加：

```yaml
mcp_servers:
  tavily:
    command: "/root/.nvm/versions/node/v24.15.0/bin/tavily-mcp"
    env:
      TAVILY_API_KEY: "your-key-here"
    timeout: 60
```

API Key 也建议放到 `~/.hermes/.env`：

```dotenv
TAVILY_API_KEY=tvly-xxx...
```

### 提供的工具

| 工具 | 功能 |
|------|------|
| `tavily_search` | 实时搜索（支持 basic/advanced/fast/ultra-fast 深度、域名过滤、时间过滤） |
| `tavily_extract` | 提取网页内容 |
| `tavily_crawl` | 网站爬取 |
| `tavily_map` | 网站地图分析 |
| `tavily_research` | 多源综合调研（mini/pro/auto） |

### 优点

- 中国服务器可用（不走 Bing/DDG）
- 返回结构化 JSON，稳定可靠
- 免费版每月 1000 次请求

---

## 3. Firecrawl CLI（CLI 工具）

### 安装

```bash
npm install -g firecrawl-cli
```

### 配置

API Key 写入 `~/.hermes/.env` 或终端环境变量：

```dotenv
FIRECRAWL_API_KEY=fc-xxx...
```

### 用法

```bash
# 抓取网页
firecrawl scrape "https://example.com"

# 搜索（输出重定向到文件）
firecrawl search "keywords" -o output.md

# 注意：用 -o 写文件时某些版本可能崩溃，可用 stdout 直接输出
firecrawl search "keywords" > output.md
```

### 优点

- CLI 工具，可直接在 terminal() 中使用
- 抓取质量高，返回干净 markdown
- 适合需要精确控制的场景

---

## 4. 三者分工

| 场景 | 用哪个 |
|------|--------|
| 会话中搜索/提取数据（主） | AnySearch MCP（默认搜索工具集） |
| 会话中搜索/提取数据（备） | Tavily MCP |
| 需要文件输出/精确控制 | Firecrawl CLI（终端命令） |
| 需要爬取整个网站 | Tavily / Firecrawl |
| 需要交互操作页面（点击、填表） | Firecrawl（interact 功能） |
| 垂直领域查询（金融/学术/法律等） | AnySearch（内置垂直搜索） |

---

## 注意事项

- **不公开敏感信息**：API Key 只放 `.env`，不进 Git/公开文档
- **中国服务器限制**：Tavily 和 AnySearch 在中国服务器上均可正常使用；Firecrawl 的搜索功能在中国也有良好表现
- 修改 config.yaml 后需要 `/reload-mcp` 或重启 Hermes 才能生效
- AnySearch 的 API Key 前缀通常为 `as_sk_`，Tavily 为 `tvly-`，Firecrawl 为 `fc-`
