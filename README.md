# Dataify MCP

[Chinese README](README.zh-CN.md)

Dataify MCP connects OpenClaw and other MCP clients to Dataify's remote data tools: web unlocking, SERP search, scraper task submission, task status, account usage, and credit/balance queries.

This package is a ClawHub bundle plugin. It does not execute local code. It registers the remote Dataify MCP server and adds guidance for when agents should use Dataify tools.

## Install

```bash
openclaw plugins install clawhub:dataify-mcp
```

Then configure your Dataify API token:

```bash
export DATAIFY_API_TOKEN="your_dataify_api_token"
```

Restart OpenClaw or the MCP host after changing environment variables.

## MCP Server

The plugin registers the `dataify` MCP server:

```yaml
mcp_servers:
  dataify:
    url: "https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}"
    enabled: true
```

If your Dataify MCP endpoint is different, update `.mcp.json` before publishing or installing the bundle.

## What You Can Do

- Unlock and render public web pages as HTML or PNG screenshots.
- Search Google, Bing, Yandex, and DuckDuckGo result data.
- Retrieve Google News, Images, Maps, Flights, Jobs, Shopping, Trends, Play, Scholar, Finance, Hotels, Patents, and Lens data.
- Retrieve Bing News, Images, Maps, Shopping, Videos, and Search data.
- Submit and inspect Dataify scraper tasks for public sources such as Amazon, YouTube, TikTok, Instagram, Facebook, LinkedIn, Reddit, Walmart, Zillow, Google Maps, Google Play, Glassdoor, Indeed, Crunchbase, Airbnb, Booking, and eBay.
- Query Dataify task status, task statistics, products, tool pricing, balance, user info, API keys, and credit usage.

## Representative Tools

The remote server exposes a large MCP tool catalog. Common tools include:

| Tool | Purpose |
| --- | --- |
| `request_web_unlocker` | Unlock a URL and return rendered HTML or a PNG screenshot. |
| `google_search` | Retrieve Google Search results with locale, language, device, cache, and rendering options. |
| `google_maps` | Retrieve Google Maps place/search results. |
| `google_news` | Retrieve Google News results and topic/publication/story data. |
| `google_images` | Retrieve Google Images search results. |
| `google_lens` | Run image-based Google Lens searches. |
| `bing_search` | Retrieve Bing Search results. |
| `bing_news` | Retrieve Bing News results. |
| `duckduckgo_search` | Retrieve DuckDuckGo search results. |
| `yandex_search` | Retrieve Yandex search results. |
| `query_common_collection_api_task_status` | Query Dataify collection task status and usage. |
| `query_common_collection_api_statistics` | Query Dataify collection statistics. |
| `query_scraper_and_serp_tasks` | Query scraper and SERP task lists. |
| `query_scraper_and_serp_products` | Query available scraper and SERP products. |
| `query_scraper_and_serp_product_tools` | Query tool lists and credit prices by product. |
| `query_user_balance` | Query remaining balance or credits. |
| `query_user_info` | Query Dataify account information. |
| `query_user_api_keys` | Query API keys for the current account. |
| `query_user_credit_usage` | Query daily credit usage. |

## Authentication

Dataify MCP uses a Dataify API token in the MCP endpoint URL:

```text
https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}
```

Treat this token like a password:

- Do not commit real token values to Git.
- Do not paste token values into public prompts, screenshots, tickets, or logs.
- Prefer environment variables or your MCP host's secret storage.
- Rotate the token if it may have been exposed.

## Credits And Remote Work

Some Dataify tools may consume account credits or submit asynchronous remote collection tasks. Use the smallest relevant tool for the user's goal and check account usage with `query_user_balance` or `query_user_credit_usage` when cost matters.

## Manual MCP Configuration

For MCP clients that do not install ClawHub bundles, configure the remote server manually:

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

## Requirements

- A Dataify account and API token.
- Network access to the Dataify MCP endpoint.
- An MCP host that supports remote Streamable HTTP MCP servers and environment variable interpolation in URLs.

## Support

Create a Dataify account or manage API tokens at:

```text
https://www.dataify.com/dashboard
```
