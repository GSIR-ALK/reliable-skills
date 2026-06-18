# 服务器搜索工具配置经验

在服务器上配置了两套搜索工具：**Tavily**（MCP 搜索服务）和 **Firecrawl**（CLI 抓取/搜索工具）。互补使用。

---

## 1. Tavily MCP

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

## 2. Firecrawl CLI

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

## 3. 两者分工

| 场景 | 用哪个 |
|------|--------|
| 会话中需要搜索/提取数据 | Tavily MCP（工具集成到会话） |
| 需要文件输出/精确控制 | Firecrawl CLI（终端命令） |
| 需要爬取整个网站 | 两者都可以，Tavily 更方便 |
| 需要交互操作页面（点击、填表） | Firecrawl（interact 功能） |

---

## 注意事项

- **不公开敏感信息**：API Key 只放 `.env`，不进 Git/公开文档
- **中国服务器限制**：Tavily 在中国可用；Firecrawl 的 `search` 命令在中国可用，无需额外配置
- 修改 config.yaml 后需要 `/reload-mcp` 或重启 Hermes 才能生效
