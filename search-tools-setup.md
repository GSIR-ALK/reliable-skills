# 服务器搜索工具配置经验

服务器上配置了三套搜索工具：**AnySearch**、**Tavily**、**Firecrawl**。每套支持不同的使用模式。

---

## 1. AnySearch — MCP（主搜索引擎）

当前作为 Hermes 主搜索 MCP 服务使用，会话中自动通过 MCP 工具调用。

### 模式：MCP ✅

**配置（`~/.hermes/config.yaml`）：**

```yaml
mcp_servers:
  anysearch:
    url: https://api.anysearch.com/mcp
    headers:
      Authorization: Bearer as_sk_xxx...
    timeout: 30
```

**工具清单：**

| 工具 | 功能 |
|------|------|
| `mcp_anysearch_search` | 主搜索（支持垂直领域路由） |
| `mcp_anysearch_batch_search` | 批量搜索（最多5条并行） |
| `mcp_anysearch_get_sub_domains` | 获取垂直搜索子领域 |
| `mcp_anysearch_extract` | 提取网页完整内容 |

**使用流程：**
- 通用查询 → 直接 `search`
- 多意图 → `batch_search` 并行
- 垂直搜索 → `get_sub_domains` → `search`（带 domain/sub_domain）
- 结果不够 → `extract` 获取全文

### 其他模式

未发现有独立的 CLI 或 SDK，仅 MCP 模式。

---

## 2. Tavily — MCP + CLI + REST

### 模式 A：MCP ✅

**安装：**
```bash
npm install -g tavily-mcp
```

**配置（`~/.hermes/config.yaml`）：**
```yaml
mcp_servers:
  tavily:
    command: /root/.nvm/versions/node/v24.15.0/bin/tavily-mcp
    env:
      TAVILY_API_KEY: "your-key-here"
    timeout: 60
```

**工具清单：**

| 工具 | 功能 |
|------|------|
| `tavily_search` | 实时搜索（basic/advanced/fast/ultra-fast 深度、域名/时间过滤） |
| `tavily_extract` | 提取网页内容 |
| `tavily_crawl` | 网站爬取（depth/breadth/limit） |
| `tavily_map` | 网站地图分析 |
| `tavily_research` | 多源综合调研（mini/pro/auto） |

### 模式 B：CLI（需 bun 运行时）

**安装：**
```bash
npm install -g tavily-cli
```

**注意：** `tavily-cli` 依赖 `bun` 运行时，如未安装 bun 则无法直接使用。可通过 `npm install -g bun` 安装 bun 后使用。

**用法：**
```bash
tavily-cli search "keywords"
tavily-cli extract "https://example.com"
tavily-cli crawl "https://example.com" --depth 2
tavily-cli map "https://example.com"
```

### 模式 C：REST API（@tavily/core）

```javascript
import Tavily from "@tavily/core";
const tvly = new Tavily({ apiKey: process.env.TAVILY_API_KEY });
const result = await tvly.search("query");
```

---

## 3. Firecrawl — CLI + MCP + REST

### 模式 A：CLI ✅（直接终端使用）

**安装：**
```bash
npm install -g firecrawl-cli
```

**配置：**
```dotenv
FIRECRAWL_API_KEY=fc-xxx...
```

**用法：**
```bash
# 抓取网页
firecrawl scrape "https://example.com"

# 搜索
firecrawl search "keywords"

# 保存输出
firecrawl search "keywords" > output.md

# 注意：-o 参数在某些版本可能崩溃，用 stdout 重定向更稳妥
firecrawl scrape "https://example.com" > page.md
```

### 模式 B：MCP ✅（会话中工具调用）

**安装：**
```bash
npm install -g firecrawl-mcp
```

**配置（`~/.hermes/config.yaml`）：**
```yaml
mcp_servers:
  firecrawl:
    command: /root/.nvm/versions/node/v24.15.0/bin/firecrawl-mcp
    env:
      FIRECRAWL_API_KEY: "fc-xxx..."
    timeout: 60
```

**工具清单：**

| 工具 | 功能 |
|------|------|
| `firecrawl_search` | 网页搜索 |
| `firecrawl_scrape` | 网页抓取 |
| `firecrawl_crawl` | 站点爬取 |
| `firecrawl_map` | 站点地图 |

**注意：** 即使不配 API Key 也能用（免费 keyless 模式，有限速），但建议配 key 以获得完整功能。

### 模式 C：REST API（v2）

```bash
# 搜索
curl -X POST https://api.firecrawl.dev/v2/search \
  -H "Authorization: Bearer fc-xxx..." \
  -H "Content-Type: application/json" \
  -d '{"query": "keywords"}'

# 抓取
curl -X POST https://api.firecrawl.dev/v2/scrape \
  -H "Authorization: Bearer fc-xxx..." \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com"}'
```

---

## 4. 场景速查

| 场景 | 推荐方式 |
|------|----------|
| 会话中搜索/提取（主） | AnySearch MCP |
| 会话中搜索/提取（备） | Tavily MCP |
| 会话中抓取网页 | Firecrawl MCP / Tavily MCP |
| 终端里搜资料 | Firecrawl CLI / Tavily CLI |
| 写代码集成搜索功能 | Firecrawl REST API / Tavily @tavily/core |
| 爬取整站 | Tavily MCP（crawl）/ Firecrawl MCP（crawl） |
| 批量调研 | Tavily MCP（research） |
| 垂直领域查询 | AnySearch（领域路由） |

---

## 注意事项

- **敏感信息保护**：API Key 只放 `.env`，不进 Git/公开文档
- **中国服务器**：Tavily 和 AnySearch 在中国可用；Firecrawl 搜索/抓取在中国也有良好表现
- 修改 `config.yaml` 后需要 `/reload-mcp` 或重启 Hermes 才能生效
- Firecrawl MCP 可 keyless 使用（有限速），适合快速测试
- Tavily CLI 需要 `bun` 运行时
