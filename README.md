# CoinPulse

CoinPulse is a Next.js 16 crypto screener that combines CoinGecko REST endpoints with WebSocket streaming to deliver live prices, trades, OHLCV candles, and market insights in a single dashboard.

## Features
- Live coin detail terminal: streaming price, recent trades, and live OHLC overlay via `useCoinGeckoWebSocket`.
- Interactive candlestick chart: multiple timeframes, live update frequency (1s/1m), and chart presets backed by `lightweight-charts`.
- Market discovery: home dashboard with Bitcoin overview, trending coins, and top categories; full coins table with pagination.
- Conversion and metadata: fiat/asset conversion widget, market cap stats, and outbound links to explorers, sites, and communities.
- Modern UI: Tailwind CSS v4, Radix primitives, lucide icons, and reusable table/pagination components.

## Tech Stack
- Next.js 16 (App Router, server components, Suspense) on React 19
- Tailwind CSS 4, Radix UI, lucide-react
- lightweight-charts for candlesticks
- SWR-style server actions with incremental caching
- WebSocket streaming from CoinGecko

## Getting Started
1. Install dependencies (Node 18.18+ recommended for Next 16):
	```bash
	npm install
	```
2. Create `.env.local`:
	```bash
	COINGECKO_BASE_URL=https://api.coingecko.com/api/v3          # or your Pro base URL
	COINGECKO_API_KEY=your_rest_api_key
	NEXT_PUBLIC_COINGECKO_WEBSOCKET_URL=wss://stream.coingecko.com/price/v1  # Pro/WebSocket endpoint
	NEXT_PUBLIC_COINGECKO_API_KEY=your_rest_api_key
	```
3. Run the app:
	```bash
	npm run dev
	```
4. Lint:
	```bash
	npm run lint
	```
5. Production build:
	```bash
	npm run build && npm run start
	```

## Project Structure
- app/: App Router pages, layout, and route segments
  - page.tsx: landing dashboard (overview, trending, categories)
  - coins/page.tsx: paginated market table
  - coins/[id]/page.tsx: coin detail with live terminal and converter
- components/: UI building blocks (candlestick chart, data tables, pagination, dialogs) and home widgets
- hooks/useCoinGeckoWebSocket.ts: streaming price/trade/OHLCV handler
- lib/coingecko.actions.ts: typed REST fetcher and pool lookup helper
- lib/utils.ts: formatting helpers and pagination builder
- constants.ts: chart presets, period and live-interval button configs
- type.d.ts: shared DTOs and component props

## Data Flow
- REST: server actions via `fetcher` with `next: { revalidate }` for cached market data, OHLC history, trending lists, and categories.
- WebSocket: subscribes to price (`CGSimplePrice`), trades (`OnchainTrade`), and OHLCV (`OnchainOHLCV`) channels; live candles are merged with historical data for seamless charting.

## Deployment
- Vercel-ready; ensure `.env.local` is provided as project environment variables.
- Run `npm run build` to validate before deploying.

## Notes
- Live data requires valid CoinGecko API keys (REST + WebSocket). Without them, pages will fail to fetch or stream.
- Pool-based streaming depends on network/contract resolution; falls back gracefully when no pool is found.
