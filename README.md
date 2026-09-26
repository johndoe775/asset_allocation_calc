# ETF Portfolio Rebalancer

A GitHub Pages frontend + Cloudflare Worker CORS proxy for calculating whole-unit ETF portfolio allocations.

## Files

- `index.html` — frontend dashboard
- `worker.js` — Cloudflare Worker proxy
- `README.md` — deployment instructions

## 1. Deploy the Cloudflare Worker

Install Wrangler if needed:

```bash
npm install -g wrangler
```

Log in:

```bash
wrangler login
```

Deploy:

```bash
wrangler deploy worker.js
```

Copy the Worker URL produced by Wrangler.

## 2. Configure the frontend

Open `index.html` and replace:

```js
const CLOUDFLARE_WORKER_URL = "https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev";
```

with your actual Worker URL.

## 3. Test locally

Because the page makes cross-origin requests, use a local HTTP server rather than opening `index.html` directly.

For example:

```bash
python -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

## 4. Deploy to GitHub Pages

Create a GitHub repository and upload:

```text
index.html
```

Then enable GitHub Pages from the repository's Pages settings.

The Worker remains on Cloudflare; the static dashboard is served by GitHub Pages.

## Portfolio configuration

The dashboard uses:

- NSE:NIFTY200EW — 40.5%
- NSE:MID150BEES — 31.5%
- NSE:HDFCSML250 — 18.0%
- NSE:TATAGOLD — 10.0%

## Allocation formula

For each asset:

```text
Target Value = Capital × Target Weight
Units to Buy = floor(Target Value / LTP)
Invested Value = Units to Buy × LTP
Unused Cash = Capital − Total Invested Value
Cash Drag = Unused Cash / Capital
```

Actual Weight shown in the table is:

```text
Actual Weight = Invested Value / Total Invested Value
```

## Important

TradingView's scanner endpoint is an internal/non-public endpoint and may change its request or response format. If TradingView changes it, the Worker or frontend parser may need to be updated.

This project only calculates allocations; it does not place trades.
