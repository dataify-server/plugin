---
name: dataify-mcp
description: Use Dataify MCP when the user explicitly needs web unlocking, search engine result data, public web/platform scraping tasks, Dataify task status, Dataify usage statistics, account balance, or API key/account information through Dataify.
---

# Dataify MCP

Use this skill when the user explicitly asks for one of these Dataify-backed actions:

- Unlock or render a public web page and return HTML or a PNG screenshot.
- Search or retrieve SERP data from Google, Bing, Yandex, or DuckDuckGo.
- Retrieve specialized public data through Dataify scraper tools, such as Amazon, YouTube, TikTok, Instagram, Facebook, LinkedIn, Reddit, Walmart, Zillow, Google Maps, Google Play, Glassdoor, Indeed, Crunchbase, Airbnb, Booking, or eBay data.
- Check Dataify task status, collection statistics, scraper/SERP products, available tools, account balance, user info, API keys, or credit usage.

Before calling tools:

1. Confirm the user has provided or configured `DATAIFY_API_TOKEN`.
2. Prefer the smallest relevant tool for the user's stated goal.
3. Do not call Dataify for ordinary reasoning, writing, coding, summarization, or browsing tasks unless the user clearly asks to use Dataify or needs Dataify-specific data.
4. Tell the user when a request may consume Dataify credits or create a remote collection task.

The remote MCP server is registered as `dataify` and authenticates through the endpoint URL:

```text
https://mcp.dataify.com/mcp?token=${DATAIFY_API_TOKEN}&tools=user_info,web_unlocker,google_serp,yandex_serp,duckduckgo_serp,bing_serp,amazon,youtube,facebook,instagram,reddit,walmart,google,booking,indeed,airbnb,google_play_store,github,tiktok,linkedin,glassdoor,twitter,crunchbase,zillow,ebay
```

The server exposes a large tool catalog. Representative tools include:

- `request_web_unlocker`
- `google_search`, `google_news`, `google_images`, `google_maps`, `google_flights`, `google_jobs`, `google_lens`
- `bing_search`, `bing_news`, `bing_images`, `bing_maps`, `bing_shopping`, `bing_videos`
- `yandex_search`, `duckduckgo_search`
- `query_common_collection_api_task_status`, `query_common_collection_api_statistics`
- `query_scraper_and_serp_tasks`, `query_scraper_and_serp_products`, `query_scraper_and_serp_product_tools`
- `query_user_balance`, `query_user_info`, `query_user_api_keys`, `query_user_credit_usage`

Treat the Dataify token like a password. Do not print it, commit it, or include it in shared logs.
