# Dataify MCP

Dataify MCP 是一个用于 ClawHub / OpenClaw 的远程 MCP Bundle 插件。它会把 OpenClaw 或其他 MCP 客户端连接到 Dataify 的远程 MCP 服务，提供网页解锁、搜索引擎结果采集、公开平台数据采集任务、任务状态查询、账号余额和积分使用查询等能力。

这个包是 **Bundle Plugin**，不会在用户本地执行插件代码。它只注册远程 Dataify MCP 服务，并附带一份 Agent 使用指引。

## 安装

```bash
openclaw plugins install clawhub:dataify-mcp
```

安装后配置 Dataify API Token：

```bash
export DATAIFY_API_TOKEN="your_dataify_api_token"
```

修改环境变量后，请重启 OpenClaw 或对应 MCP Host。

## MCP 服务配置

插件会注册名为 `dataify` 的远程 MCP 服务：

```yaml
mcp_servers:
  dataify:
    url: "https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}"
    enabled: true
```

如果正式 MCP 地址后续变更，只需要同步修改 `.mcp.json` 中的 `url`。

## 可用能力

- 解锁并渲染公开网页，返回 HTML 或 PNG 截图。
- 获取 Google、Bing、Yandex、DuckDuckGo 等搜索结果数据。
- 获取 Google News、Images、Maps、Flights、Jobs、Shopping、Trends、Play、Scholar、Finance、Hotels、Patents、Lens 等数据。
- 获取 Bing News、Images、Maps、Shopping、Videos、Search 等数据。
- 提交和查询 Dataify 公开平台采集任务，例如 Amazon、YouTube、TikTok、Instagram、Facebook、LinkedIn、Reddit、Walmart、Zillow、Google Maps、Google Play、Glassdoor、Indeed、Crunchbase、Airbnb、Booking、eBay 等。
- 查询 Dataify 任务状态、统计数据、产品列表、工具价格、账号余额、用户信息、API Key 和积分使用情况。

## 常用工具

远程 MCP 服务会暴露较多工具，常见工具包括：

| 工具 | 用途 |
| --- | --- |
| `request_web_unlocker` | 解锁 URL，返回渲染后的 HTML 或 PNG 截图。 |
| `google_search` | 获取 Google Search 结果，支持国家、语言、设备、缓存和渲染参数。 |
| `google_maps` | 获取 Google Maps 地点或搜索结果。 |
| `google_news` | 获取 Google News 结果、主题、出版物和报道数据。 |
| `google_images` | 获取 Google Images 搜索结果。 |
| `google_lens` | 基于图片 URL 执行 Google Lens 搜索。 |
| `bing_search` | 获取 Bing Search 结果。 |
| `bing_news` | 获取 Bing News 结果。 |
| `duckduckgo_search` | 获取 DuckDuckGo 搜索结果。 |
| `yandex_search` | 获取 Yandex 搜索结果。 |
| `query_common_collection_api_task_status` | 查询 Dataify 通用采集 API 任务状态和消耗。 |
| `query_common_collection_api_statistics` | 查询 Dataify 通用采集 API 统计数据。 |
| `query_scraper_and_serp_tasks` | 查询网页采集和 SERP 任务列表。 |
| `query_scraper_and_serp_products` | 查询可用的网页采集和 SERP 产品。 |
| `query_scraper_and_serp_product_tools` | 按产品查询工具列表和积分价格。 |
| `query_user_balance` | 查询账号剩余余额或积分。 |
| `query_user_info` | 查询 Dataify 账号信息。 |
| `query_user_api_keys` | 查询当前账号的 API Key。 |
| `query_user_credit_usage` | 查询每日积分使用情况。 |

## 鉴权

Dataify MCP 使用 URL 查询参数中的 Dataify API Token 鉴权：

```text
https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}
```

请把 API Token 当作密码处理：

- 不要把真实 Token 提交到 Git。
- 不要粘贴到公开 Prompt、截图、工单或日志里。
- 优先使用环境变量或 MCP Host 的密钥存储能力。
- 如果 Token 可能已经泄露，请及时轮换。

## 积分和远程任务

部分 Dataify 工具可能会消耗账号积分，或提交异步远程采集任务。使用时建议选择最贴近需求的工具；如果成本敏感，可以通过 `query_user_balance` 或 `query_user_credit_usage` 查询余额和积分使用情况。

## 手动 MCP 配置

如果 MCP 客户端不支持 ClawHub Bundle，也可以手动添加远程 MCP：

```json
{
  "mcpServers": {
    "dataify": {
      "type": "http",
      "transport": "streamable-http",
      "url": "https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}"
    }
  }
}
```

## 使用要求

- 拥有 Dataify 账号和 API Token。
- 当前网络可以访问 Dataify MCP 服务地址。
- MCP Host 支持远程 Streamable HTTP MCP，并支持在 URL 中使用环境变量。

## 发布检查

ClawHub 要求插件发布包包含 `openclaw.plugin.json`，并提供源码仓库和精确提交记录。推荐从公开 GitHub 仓库 checkout 后发布；如果从本地目录发布，需要在 CLI 中显式传入 source metadata。

```bash
clawhub package publish ./clawhub-package-preview \
  --family bundle-plugin \
  --source-repo <public-owner/public-repo> \
  --source-commit <exact-commit-sha> \
  --source-ref <branch-or-tag> \
  --source-path clawhub-package-preview \
  --dry-run
```

Dry run 通过后，再去掉 `--dry-run` 正式发布。

`--source-commit` 应该填写包含当前 Bundle 文件的那次提交，也就是已经提交了 `README.md`、`README.zh-CN.md`、`package.json`、`openclaw.plugin.json`、`.mcp.json`、`.claude-plugin/plugin.json` 和 `skills/dataify-mcp/SKILL.md` 的公开仓库 commit。

## 获取 API Token

可在 Dataify 控制台创建或管理 API Token：

```text
https://www.dataify.com/dashboard
```
