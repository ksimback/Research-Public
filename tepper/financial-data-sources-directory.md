# Financial & Crypto Market Data Sources Directory

**Compiled:** February 13, 2026 | **Updated:** March 25, 2026
**Purpose:** Comprehensive directory of free/low-cost market data APIs for portfolio management
**Focus:** Free tiers, rate limits, data coverage, fallback strategies

---

## Table of Contents

1. [Macro & Economic Data](#1-macro--economic-data)
2. [Treasury & Rates](#2-treasury--rates)
3. [Foreign Exchange (FX)](#3-foreign-exchange-fx)
4. [Equities](#4-equities)
5. [Crypto — Price & Market Data](#5-crypto--price--market-data)
6. [Crypto — Derivatives & Perpetuals](#6-crypto--derivatives--perpetuals)
7. [Crypto — DeFi & On-Chain](#7-crypto--defi--on-chain)
8. [Crypto — On-Chain Analytics (Paid)](#8-crypto--on-chain-analytics-paid)
9. [News & Sentiment](#9-news--sentiment)
10. [Multi-Asset Toolkits](#10-multi-asset-toolkits)
11. [Charting & Visualization](#11-charting--visualization)
12. [API Budget & Rotation Strategy](#12-api-budget--rotation-strategy)
13. [Fallback Hierarchies](#13-fallback-hierarchies)
14. [Implementation Priority](#14-implementation-priority)
15. [Sources Evaluated but Not Implemented](#15-sources-evaluated-but-not-implemented)

---

## 1. Macro & Economic Data

### FRED API (Federal Reserve Economic Data)
| Field | Details |
|-------|---------|
| **Provider** | Federal Reserve Bank of St. Louis |
| **Cost** | Free (API key required) |
| **Rate Limits** | 120 requests/minute |
| **Coverage** | 800,000+ economic data series — GDP, CPI/PCE, employment, interest rates, monetary policy, M2 money supply, Fed balance sheet, regional data |
| **Key Series** | `WALCL` (Fed balance sheet), `CPIAUCSL` (CPI), `UNRATE` (unemployment), `M2SL` (M2 supply), `FEDFUNDS` (fed funds rate) |
| **Python Library** | `fredapi` |
| **Docs** | https://fred.stlouisfed.org/docs/api/fred/ |
| **Best For** | Primary source for all US macro data |

### Bureau of Labor Statistics (BLS) API
| Field | Details |
|-------|---------|
| **Provider** | U.S. Bureau of Labor Statistics |
| **Cost** | Free (no registration required) |
| **Rate Limits** | Generous (not officially documented) |
| **Coverage** | CPI (inflation), Employment Situation (NFP), unemployment rates, wage data, productivity statistics |
| **Docs** | https://www.bls.gov/developers/api_signature_v2.htm |
| **Best For** | Backup/alternative to FRED for employment and inflation data |

### Trading Economics
| Field | Details |
|-------|---------|
| **Provider** | Trading Economics |
| **Cost** | Free guest tier (limited); paid plans available |
| **Coverage** | Economic calendar, macro indicators, forecasts for 196 countries |
| **Python Library** | `tradingeconomics` |
| **Website** | https://tradingeconomics.com |
| **Best For** | Economic event calendar, global macro indicators |

---

## 2. Treasury & Rates

### U.S. Treasury Fiscal Data API
| Field | Details |
|-------|---------|
| **Provider** | U.S. Department of the Treasury |
| **Cost** | Free |
| **Rate Limits** | Generous (not specified) |
| **Coverage** | Daily Treasury yield curve rates, bills/notes/bonds, debt to the penny, interest rate statistics |
| **Docs** | https://fiscaldata.treasury.gov/api-documentation/ |
| **Best For** | Primary source for yield curve and Treasury data |

### FRED API (Treasury Series)
| Series ID | Description |
|-----------|-------------|
| `DGS1MO` | 1-Month Treasury Constant Maturity Rate |
| `DGS3MO` | 3-Month Treasury Constant Maturity Rate |
| `DGS2` | 2-Year Treasury Constant Maturity Rate |
| `DGS10` | 10-Year Treasury Constant Maturity Rate |
| `DGS30` | 30-Year Treasury Constant Maturity Rate |
| `T10Y2Y` | 10-Year minus 2-Year Treasury Spread |

---

## 3. Foreign Exchange (FX)

### ExchangeRate-API / Fixer.io
| Field | Details |
|-------|---------|
| **Cost** | Free tier (~1,000 requests/month) |
| **Coverage** | 170+ currencies, real-time and historical |
| **Best For** | Primary FX data source on free tier |

### FRED API (FX Series)
| Series ID | Description |
|-----------|-------------|
| `DEXUSEU` | USD/EUR exchange rate |
| `DEXUSUK` | USD/GBP exchange rate |
| `DEXUSJP` | USD/JPY exchange rate |
| `DEXCHUS` | CNY/USD exchange rate |

### Alpha Vantage (FX)
- Free tier: 25 requests/day
- Covers physical and digital/crypto currencies
- Use as fallback for exotic pairs

---

## 4. Equities

### Yahoo Finance (yfinance)
| Field | Details |
|-------|---------|
| **Cost** | Free (unofficial API via scraping) |
| **Rate Limits** | ~1,000-2,000 requests/day (practical limit) |
| **Coverage** | Global stocks, ETFs, mutual funds, crypto, forex |
| **Historical Data** | Full history available |
| **Intraday Data** | 1min, 2min, 5min, 15min, 30min, 60min, 90min |
| **Real-time** | 15-minute delayed for most exchanges |
| **Python Library** | `yfinance` |
| **Best For** | Primary equities data source (free tier) |
| **Risk** | Unofficial API — may break without warning. Implement retry logic and caching. |

### Polygon.io
| Field | Details |
|-------|---------|
| **Cost** | Free tier (5 API calls/minute); paid: $199-$499/mo |
| **Coverage** | US stocks, options, forex, crypto |
| **Free Tier Limits** | End-of-day data only, 2+ years historical, no real-time intraday, no WebSocket |
| **Free Endpoints** | Aggregates (OHLCV), previous close, daily open/close, ticker details |
| **Docs** | https://polygon.io/docs |
| **Best For** | Historical data, backtesting, EOD snapshots |

### Alpha Vantage
| Field | Details |
|-------|---------|
| **Cost** | Free tier: 25 requests/day, 5/minute; paid: $49.99-$599.99/mo |
| **Coverage** | US stocks, ETFs, forex, crypto, technical indicators (RSI, MACD, etc.) |
| **Historical Data** | 20+ years daily data |
| **Intraday** | 1min-60min (premium for higher freq) |
| **Docs** | https://www.alphavantage.co/documentation/ |
| **Best For** | Technical indicators, backup equity quotes |

### Financial Modeling Prep (FMP)
| Field | Details |
|-------|---------|
| **Cost** | Free tier: 500MB bandwidth/30 days; paid: $19-$199/mo |
| **Coverage** | Stocks, forex, crypto, commodities, financial statements, ratios |
| **Free Endpoints** | 30+ endpoints, 5+ years historical |
| **Docs** | https://site.financialmodelingprep.com/developer/docs |
| **Best For** | Fundamental analysis, financial statements, company ratios |

---

## 5. Crypto — Price & Market Data

### CoinGecko
| Field | Details |
|-------|---------|
| **Cost** | Free demo tier: 30 calls/min, 10,000 calls/month (API key required since Feb 2024) |
| **Coverage** | 10,000+ coins, 800+ exchanges |
| **Endpoints** | 50+ — prices, market cap, volume, historical charts, global metrics, trending |
| **Key Endpoints** | `/coins/markets`, `/coins/{id}/market_chart`, `/global`, `/exchanges` |
| **Docs** | https://www.coingecko.com/en/api/documentation |
| **Best For** | Primary source for crypto spot prices and market data |

### CoinMarketCap
| Field | Details |
|-------|---------|
| **Cost** | Free tier: 10,000 call credits/month, 30 req/min; paid: $29/mo+ |
| **Coverage** | 10,000+ cryptocurrencies, 400+ exchanges |
| **Free Tier Limitation** | No historical data on free tier |
| **Free Endpoints** | `/cryptocurrency/listings/latest`, `/cryptocurrency/quotes/latest`, `/global-metrics/quotes/latest` |
| **Docs** | https://coinmarketcap.com/api/documentation |
| **Best For** | Rankings, current prices, exchange volume data |

### Messari
| Field | Details |
|-------|---------|
| **Cost** | Free: 20 requests/minute, no API key for basic endpoints; paid: $29/mo+ |
| **Coverage** | 5,000+ crypto assets, qualitative metrics, governance data |
| **Endpoints** | Assets, markets, metrics, news |
| **API Base** | `https://data.messari.io/api/v1/` |
| **Best For** | Asset metadata, qualitative analysis, governance data |

---

## 6. Crypto — Derivatives & Perpetuals

### TrueNorth CLI
| Field | Details |
|-------|---------|
| **Cost** | Included with TrueNorth subscription |
| **Coverage** | Crypto technical analysis, derivatives (funding rates, open interest), liquidation risk, DeFi metrics, token unlocks, events calendar, performance scanning |
| **Key Commands** | `tn technical-analysis`, `tn derivatives-analysis`, `tn liquidation-risk`, `tn performance-scanner`, `tn events`, `tn token-unlock`, `tn meme-discovery` |
| **Data Sources** | Aggregates from Binance, Bybit, Hyperliquid, and more |
| **Best For** | All-in-one crypto derivatives intelligence — funding rates, OI percentiles, liquidation maps, TA indicators (RSI, MACD, Bollinger, Stochastic, ADX) |

### Hyperliquid API
| Field | Details |
|-------|---------|
| **Cost** | Free — no API key required |
| **Rate Limits** | Not officially documented; be respectful |
| **Coverage** | 100+ perpetual contracts, spot assets, vaults |
| **WebSocket** | Yes |
| **Base URL** | `https://api.hyperliquid.xyz/` |
| **Key Endpoints** | `POST /info` — `allMids` (prices), `meta` (asset metadata), `openOrders` (order book), `fundingHistory` |
| **Python SDK** | `hyperliquid-python-sdk` |
| **Data Available** | Real-time L2 order book, mark prices, funding rates (8h), liquidation data, open interest, 24h volume, vault performance |
| **Best For** | Primary source for perp-specific data — no limits, real-time |

### CCXT (CryptoCurrency eXchange Trading)
| Field | Details |
|-------|---------|
| **Cost** | Free (open-source library) |
| **Coverage** | 108+ cryptocurrency exchanges via unified API |
| **Exchanges Tested** | Binance, Hyperliquid |
| **Python Library** | `ccxt` |
| **Docs** | https://docs.ccxt.com |
| **Best For** | Multi-exchange data access, unified interface for OHLCV, order books, funding rates across all major exchanges |

---

## 7. Crypto — DeFi & On-Chain

### DeFi Llama
| Field | Details |
|-------|---------|
| **Cost** | Free — open API, no limits specified; Pro: $300/mo |
| **Coverage** | TVL, yields, volumes, fees, stablecoins across 200+ chains and 7,000+ protocols |
| **Key Endpoints** | `/protocols` (TVL), `/charts` (historical TVL), `/coins/prices` (token prices), `/yields` (yield farming), `/stablecoins` |
| **Docs** | https://api-docs.defillama.com/ |
| **Best For** | Primary DeFi metrics — TVL tracking, protocol analysis, stablecoin supply |

### Dune Analytics
| Field | Details |
|-------|---------|
| **Cost** | Free tier: 2,500 credits/month (includes API); paid plans available |
| **Coverage** | User-generated SQL queries across 20+ chains |
| **API Base** | `https://api.dune.com/api/v1/` |
| **Docs** | https://docs.dune.com |
| **Best For** | Custom on-chain analysis — requires SQL knowledge, credits are precious so cache results |

---

## 8. Crypto — On-Chain Analytics (Paid)

These were evaluated but require paid subscriptions for API access:

### Glassnode
| Field | Details |
|-------|---------|
| **Cost** | No free API tier — $29/mo to $799/mo |
| **Coverage** | 100+ on-chain metrics for BTC, ETH, major assets |
| **Metrics** | Exchange flows, holder behavior, derivatives, fees |
| **Free Alternative** | Glassnode Studio (limited free charts, no API) |

### CryptoQuant
| Field | Details |
|-------|---------|
| **Cost** | No free API — $199/mo+ for API access |
| **Coverage** | Bitcoin, Ethereum, stablecoins, altcoins |
| **Metrics** | Exchange flows, miner data, fund flows, derivatives |
| **Free Alternative** | Limited chart access on web platform |

### Santiment
| Field | Details |
|-------|---------|
| **Cost** | No free API — $350/mo (80K API calls/mo) |
| **Coverage** | 2,000+ assets — on-chain, social, dev activity |
| **Metrics** | Network activity, holder distribution, social sentiment |
| **GraphQL API** | `https://api.santiment.net/graphql` |
| **Free Alternative** | Limited Sanbase platform access |

---

## 9. News & Sentiment

### RSS Feeds — Financial News

| Source | RSS URL | Ticker Support | Focus |
|--------|---------|----------------|-------|
| **CNBC** | `https://www.cnbc.com/id/100003114/device/rss/rss.html` | Yes | Breaking news, markets |
| **MarketWatch** | `http://feeds.marketwatch.com/marketwatch/topstories/` | Yes | Market analysis |
| **Finviz** | `https://finviz.com/news.ashx` | Yes | News aggregator |
| **Seeking Alpha** | `https://seekingalpha.com/feed.xml` | Yes | Earnings, analysis |
| **Yahoo Finance** | Ticker-specific feeds | Yes | Company news |
| **Reuters** | `https://www.reuters.com/tools/rss` | Limited | Global finance |
| **Financial Times** | `https://www.ft.com/rss` | Limited | Premium content |
| **Wall Street Journal** | `https://feeds.a.dj.com/rss/WSJcomUSBusiness.xml` | No | Paywall limited |
| **Nasdaq** | `https://www.nasdaq.com/nasdaq-RSS-Feeds` | Yes | Market-specific |
| **Zacks** | `https://www.zacks.com/rss/` | Yes | Earnings, ratings |

### RSS Feeds — Crypto News

| Source | RSS URL | Focus |
|--------|---------|-------|
| **CoinDesk** | `https://www.coindesk.com/feed` | General crypto news |
| **Cointelegraph** | `https://cointelegraph.com/rss` | Breaking news |
| **Bitcoin Magazine** | `https://bitcoinmagazine.com/feed` | Bitcoin focus |
| **Decrypt** | `https://decrypt.co/feed` | General crypto |
| **CryptoSlate** | `https://cryptoslate.com/feed/` | News + data |

**Integration:** Python `feedparser` library. Poll every 5-15 minutes. Cache aggressively.

### X/Twitter Research
| Field | Details |
|-------|---------|
| **Method** | X API (Basic tier, ~$100/mo) or manual via x-research skill |
| **Coverage** | Real-time CT (Crypto Twitter) sentiment, dev discussions, breaking news |
| **Best For** | Sentiment analysis, narrative tracking, real-time market pulse |

### Brave Web Search
| Field | Details |
|-------|---------|
| **Method** | Brave Search API |
| **Best For** | General-purpose web search for news, data, and context not covered by specialized APIs |

---

## 10. Multi-Asset Toolkits

### CCXT
- 108+ exchanges unified under one Python library
- Handles OHLCV, order books, tickers, funding rates
- Works across Binance, Bybit, Hyperliquid, Coinbase, Kraken, etc.
- Open source: https://github.com/ccxt/ccxt

### Polygon.io
- Covers equities, options, forex, AND crypto under one API
- Best for historical cross-asset analysis
- Free tier limited to EOD data

---

## 11. Charting & Visualization

### TradingView (via Browser Automation)
| Field | Details |
|-------|---------|
| **Method** | Browser relay extension + automation (screenshot capture) |
| **Capabilities** | 100+ technical indicators, custom Pine Script, multi-timeframe analysis, social sentiment, economic calendar overlay |
| **Data Export** | CSV export (Pro+ plan or Chrome extensions) |
| **Limitation** | No official API — visual analysis via screenshots only (ToS compliant) |
| **Best For** | Visual chart analysis, pattern recognition via AI vision |

---

## 12. API Budget & Rotation Strategy

### Daily/Monthly Call Budgets (Free Tiers)

| Source | Daily Budget | Monthly Budget | Priority |
|--------|-------------|----------------|----------|
| FRED API | 1,000 | 30,000 | HIGH |
| Yahoo Finance | 1,000 | ~30,000 | HIGH |
| DeFi Llama | Unlimited | Unlimited | HIGH |
| Hyperliquid | Unlimited | Unlimited | HIGH |
| TrueNorth CLI | Unlimited | Unlimited | HIGH |
| BLS API | 500 | ~15,000 | MEDIUM |
| Messari | 400 | ~12,000 | MEDIUM |
| CoinGecko | 300 | 10,000 | HIGH |
| Polygon.io | 200 | ~6,000 | MEDIUM |
| Dune | ~80 | 2,500 credits | LOW |
| Alpha Vantage | 25 | 750 | LOW (backup) |

### Time-Based Rotation

| Window | Focus |
|--------|-------|
| Pre-Market (4:00-9:30 AM ET) | Heavy equity data pull |
| Market Open (9:30 AM ET) | Real-time price updates (Hyperliquid, yfinance) |
| Intraday | Macro data updates (FRED), news monitoring |
| Market Close (4:00 PM ET) | EOD data aggregation |
| After Hours | Batch historical downloads (Polygon, yfinance) |

### Caching Strategy

| Data Type | Cache Duration |
|-----------|---------------|
| Static data (ticker lists, metadata) | 24 hours |
| Price data | 1-5 minutes |
| Macro data | 1-24 hours |
| News | 15 minutes |

---

## 13. Fallback Hierarchies

### Crypto Prices
1. TrueNorth CLI / Hyperliquid (perps) / CoinGecko (spot) — Primary
2. CoinMarketCap — Secondary
3. Messari — Tertiary
4. Yahoo Finance (crypto) — Fallback

### Equity Prices
1. Yahoo Finance — Primary
2. Financial Modeling Prep — Secondary
3. Alpha Vantage — Tertiary (strict limits)
4. Polygon.io — Bulk historical only

### Macro Data
1. FRED API — Primary
2. BLS API — Secondary (employment/inflation)
3. Treasury API — Treasury-specific
4. Trading Economics — Calendar/global indicators

### DeFi / On-Chain
1. DeFi Llama — TVL, yields, stablecoins
2. Dune Analytics — Custom SQL queries
3. Glassnode / CryptoQuant — Paid only

### News & Sentiment
1. RSS feeds (CNBC, MarketWatch, CoinDesk, etc.)
2. X/Twitter (via x-research skill or API)
3. Finviz aggregator
4. Brave web search

---

## 14. Implementation Priority

### Tier 1 — High Value, Low Complexity (Implement First)

| # | Source | Why |
|---|--------|-----|
| 1 | FRED API | Free, generous limits, comprehensive macro data |
| 2 | Yahoo Finance (yfinance) | Free, broad coverage, easy integration |
| 3 | DeFi Llama | Free, unlimited, essential DeFi data |
| 4 | Hyperliquid | Free, no limits, best perp data |
| 5 | CoinGecko | Free tier, comprehensive crypto coverage |
| 6 | TrueNorth CLI | All-in-one crypto derivatives intelligence |

### Tier 2 — Medium Complexity

| # | Source | Why |
|---|--------|-----|
| 7 | RSS Feeds | Free, essential for news/sentiment |
| 8 | BLS API | Free, employment/inflation backup |
| 9 | Treasury API | Free, best yield curve source |
| 10 | CCXT | Multi-exchange unified access |
| 11 | Dune Analytics | Free tier, custom on-chain queries |
| 12 | Messari | Free tier, asset metadata |

### Tier 3 — Lower Priority or Higher Complexity

| # | Source | Why |
|---|--------|-----|
| 13 | Polygon.io | Rate limits, mainly historical |
| 14 | Alpha Vantage | Very limited free tier (25/day) |
| 15 | Trading Economics | Guest tier limited |
| 16 | CoinMarketCap | No historical on free tier |
| 17 | TradingView | Requires browser automation |

### Tier 4 — Paid Upgrades (When Revenue Justifies)

| # | Source | Cost | Trigger |
|---|--------|------|---------|
| 1 | Coinglass API | ~$50/mo | Derivatives backtesting |
| 2 | Glassnode | $39/mo | Hourly on-chain metrics |
| 3 | Alpha Vantage Premium | $49/mo | >25 calls/day needed |
| 4 | CoinGecko Analyst | $129/mo | >10K calls/month |
| 5 | CryptoQuant | $29/mo | Real-time whale alerts |
| 6 | Santiment | $49/mo | Social sentiment alpha proven |

---

## 15. Sources Evaluated but Not Implemented

| Source | Reason Skipped |
|--------|---------------|
| **Glassnode** | No free API tier ($29-$799/mo) |
| **CryptoQuant** | No free API ($199/mo+) |
| **Santiment** | Expensive API ($350/mo) |
| **Nansen** | Premium only ($150/mo+) |
| **The Block** | No public API, scraping required |
| **Coinglass** | Paid API (~$50/mo) — good for derivatives backtesting when needed |
| **Deribit API** | Evaluated for BTC/ETH options chains — not yet integrated |
| **CryptoCompare** | Evaluated for social + market data — overlaps with existing sources |

---

## Quick Reference — API Key Requirements

| Source | Key Required | Free Key Available |
|--------|:---:|:---:|
| FRED | Yes | Yes |
| BLS | No | — |
| Treasury | No | — |
| Trading Economics | Optional | — |
| Alpha Vantage | Yes | Yes |
| Polygon.io | Yes | Yes |
| Yahoo Finance | No | — |
| FMP | Yes | Yes |
| CoinGecko | Yes | Yes |
| CoinMarketCap | Yes | Yes |
| Messari | No (basic) | — |
| DeFi Llama | No | — |
| Hyperliquid | No | — |
| Dune | Yes | Yes |
| CCXT | No | — |
| TrueNorth CLI | Subscription | — |

---

## Python Libraries Used

| Library | Purpose |
|---------|---------|
| `fredapi` | FRED economic data |
| `yfinance` | Yahoo Finance equities |
| `pycoingecko` | CoinGecko crypto data |
| `ccxt` | Multi-exchange crypto toolkit |
| `feedparser` | RSS news feeds |
| `pandas` | Data manipulation |
| `numpy` | Numerical analysis |
| `tradingeconomics` | Economic calendar |
| `requests` | HTTP calls to REST APIs |

---

*Originally compiled February 13, 2026. Updated March 25, 2026.*
*For hedge fund portfolio management — covering macro, equities, crypto, derivatives, DeFi, and news.*
