# Personal Stock Dashboard

A single-file (`index.html`) dark-mode fintech dashboard for tracking a £15,000 **Core / Satellite** portfolio, with live quotes from Yahoo Finance via RapidAPI.

| Segment | Target | Assets |
|---|---|---|
| Core (conservative) | 70% · £10,500 | Apple (AAPL), Broadcom (AVGO) |
| Satellite (high-risk) | 30% · £4,500 | Applied Digital (APLD), Sezzle (SEZL) |

## Run it
Open `index.html` in a browser. No build step is needed. The only external scripts are Tailwind CSS and Lucide Icons, loaded from CDNs.

## Features
- **Allocation calculator**: Core/Satellite slider with 60/40, 70/30 and 80/20 presets, plus per-segment ticker weights. A two-ring SVG donut shows the split. Order sizing works in fractional shares (4 d.p.) or whole shares; in whole-share mode, a residual sweep spends leftover cash on more shares. All money is computed in integer pence. Share counts round down and costs round up, so a plan never exceeds your capital. "Apply plan to watchlist" copies the calculated orders into your holdings.
- **Live watchlist**: editable ticker, segment, shares, purchase price and market price. Typing a market price overrides the API price, and a sync button reverts it. For each position the table shows value, gain/loss (£ and %) and allocation weight. Totals and actual-vs-target drift appear below the table.
- **News & Trend Stream**: a simulated, high-signal news feed for the four assets plus macro items. "Add Macro Note" saves your own notes in localStorage.
- **GBP/USD conversion**: the rate is fetched as `GBPUSD=X` in the same API call, and you can edit it manually.

## API setup
1. Subscribe to **Yahoo Finance** (apidojo) on RapidAPI.
2. Click **API Settings** and paste your key. The key is stored only in this browser's `localStorage`.

The default request is:

```
GET https://apidojo-yahoo-finance-v1.p.rapidapi.com/market/v2/get-quotes?region=US&symbols=AAPL,AVGO,APLD,SEZL,GBPUSD%3DX
x-rapidapi-key:  <your key>
x-rapidapi-host: apidojo-yahoo-finance-v1.p.rapidapi.com
```

The dashboard parses `quoteResponse.result[]` and uses three fields: `regularMarketPrice`, `regularMarketChangePercent` and `shortName`. You can change the host, path and region in the settings modal to use any RapidAPI Yahoo provider that returns the same response shape.

- **Rate limiting**: each quote is cached in localStorage for 5 minutes. Only stale or missing symbols trigger an API call.
- **Fallbacks**: if no key is set or a request fails, the dashboard uses the last cached quotes. If there are none, it uses built-in fallback prices (e.g. AAPL ≈ $333).

*For personal tracking only. Not investment advice.*
