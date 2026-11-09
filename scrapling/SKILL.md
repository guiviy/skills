# Scrapling — Python Web Scraping Framework

Source: github.com/D4Vinci/Scrapling (BSD-3-Clause). A Python framework that covers the whole scraping workflow in one library: fast HTTP requests, browser rendering for JavaScript pages, powerful parsing, concurrent crawling, and built-in export — plus a CLI, an interactive shell, an MCP server, and an Agent Skill for AI agents.

## What it is

- One library for the full pipeline: fetch a page, parse it with CSS/XPath/text/regex selectors, follow links, crawl a whole site, and export JSON/JSONL/CSV/XML.
- Three fetch layers, from lightest to heaviest: plain HTTP (`Fetcher`), browser automation for JavaScript-rendered pages (`StealthyFetcher`, Playwright Chromium / Chrome), and persistent sessions (`FetcherSession` / `StealthySession`) that keep cookies and state across requests.
- Spiders: a Scrapy-style crawling framework with async `parse` callbacks, concurrent requests, per-domain throttling, pause/resume via `crawldir`, and ready-made templates (`CrawlSpider`, `SitemapSpider`, `XMLFeedSpider` / `CSVFeedSpider`, `ShopifySpider`, `ContentSpider`).
- Parsing is the same API everywhere: `page.css()`, `page.xpath()`, `page.find_all()`, `page.find_by_text()`, chained selectors, parent/sibling navigation, and `find_similar()` to relocate elements after a site changes.
- Turn any page into clean, RAG-ready Markdown with one call (`page.markdown()`), or crawl a whole site into a Markdown corpus with the `ContentSpider` template — no LLM needed.

## When to use it

- Collecting public data for research: prices, listings, articles, documentation, datasets, or trend material for a video or article.
- Turning a website into a clean Markdown corpus for retrieval or reference.
- Building a repeatable crawler for a public source that updates regularly, with pause/resume and scheduled re-runs.
- Quick one-off extraction from the terminal without writing a script, via the CLI.
- Parsing HTML you already have: the parser works standalone (`from scrapling.parser import Selector`) without fetching anything.

## Install

- Requires Python 3.10 or higher.
- Parser only: `pip install scrapling`
- Fetchers and spiders (what most uses need): `pip install "scrapling[fetchers]"` then `scrapling install` (installs browser dependencies; add `--force` to reinstall).
- Interactive shell: `pip install "scrapling[shell]"` then `scrapling shell`
- Docker image with all browsers included is published with each release.
- Full documentation: [scrapling.readthedocs.io](https://scrapling.readthedocs.io)

## Core usage

- Fetch one page and extract with CSS selectors:

```python
from scrapling.fetchers import Fetcher

page = Fetcher.get('https://quotes.toscrape.com/')
quotes = page.css('.quote .text::text').getall()
```

- Keep a session across requests (cookies, state, browser-like TLS fingerprint):

```python
from scrapling.fetchers import FetcherSession

with FetcherSession(impersonate='chrome') as session:
    page = session.get('https://quotes.toscrape.com/', stealthy_headers=True)
    quotes = page.css('.quote .text::text').getall()
```

- Parse with several selector styles; they all return the same element objects:

```python
quotes = page.css('.quote')                                  # CSS
quotes = page.xpath('//div[@class="quote"]')                  # XPath
quotes = page.find_all('div', {'class': 'quote'})            # BeautifulSoup-style
quotes = page.find_by_text('Albert Einstein', first_match=False)  # by text
author = page.css('.quote')[0].css('.author::text').get()    # chained selectors
similar = page.css('.quote')[0].find_similar()               # similar elements
```

- Turn a page into Markdown: `page.markdown()`
- Fetch a JavaScript-rendered page with a headless browser:

```python
from scrapling.fetchers import StealthyFetcher

page = StealthyFetcher.fetch('https://quotes.toscrape.com/js/')
data = page.css('.quote .text::text').getall()
```

- Crawl a whole site with a spider, following pagination and exporting results:

```python
from scrapling.spiders import Spider, Response

class QuotesSpider(Spider):
    name = "quotes"
    start_urls = ["https://quotes.toscrape.com/"]
    concurrent_requests = 10

    async def parse(self, response: Response):
        for quote in response.css('.quote'):
            yield {
                "text": quote.css('.text::text').get(),
                "author": quote.css('.author::text').get(),
            }
        next_page = response.css('.next a')
        if next_page:
            yield response.follow(next_page[0].attrib['href'])

result = QuotesSpider().start()
result.items.to_json("quotes.json")
```

- Pause and resume a long crawl: `QuotesSpider(crawldir="./crawl_data").start()` — Ctrl+C saves pending requests and seen URLs; restart with the same `crawldir` to resume.
- Ready-made templates skip the boilerplate, e.g. a whole public Shopify store catalog:

```python
from scrapling.spiders import ShopifySpider

class MyStore(ShopifySpider):
    target_website = "example.com"

result = MyStore().start()  # every product, one item per variant
```

- Extract from the terminal without code: `scrapling extract get 'https://quotes.toscrape.com/' quotes.md` (use `.txt` for text, `.md` for Markdown, `.html` for HTML; `--css-selector '.quote'` to pick content).

## Compliance — read before scraping anything

- Only scrape data you have the right to collect: public pages, your own sites, or sources whose terms allow it. Never scrape login-walled content, personal data, or copyrighted material for redistribution.
- Respect `robots.txt`: Scrapling supports a `robots_txt_obey` flag that honors `Disallow`, `Crawl-delay`, and `Request-rate` — turn it on unless you have a specific reason not to.
- Respect the site's terms of service and rate limits: keep concurrency and delays reasonable, use AutoThrottle and per-domain throttling, and prefer the site's official API whenever one exists.
- Do not use this skill to bypass anti-bot protection, solve CAPTCHAs, or circumvent access controls. If a site blocks automated access, stop and use its official API or ask for permission — do not try to work around the block.
- Cache responses during development (Development Mode) so you iterate on parsing logic without re-hitting the target server.
- This skill only documents usage. It does not install Scrapling on the user's machine, does not run crawls by itself, and does not spend money on proxies or paid services.

## Notes and pitfalls

- `pip install scrapling` alone installs only the parser: importing anything from `scrapling.fetchers` or `scrapling.spiders` raises `ModuleNotFoundError` until you install `scrapling[fetchers]` and run `scrapling install`.
- Start with the lightest fetcher that works: plain HTTP `Fetcher` first, browser `StealthyFetcher` only for pages that genuinely need JavaScript rendering — browsers are far slower and heavier.
- Parse callbacks are async (`async def parse`); spider entry is synchronous (`Spider().start()`).
- Use `crawldir` for any crawl that might take a while or be interrupted; without it, a stopped crawl restarts from zero.
- Prefer the ready-made spider templates (`CrawlSpider`, `SitemapSpider`, `ContentSpider`) over hand-rolling link-following logic.
