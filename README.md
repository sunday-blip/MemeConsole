## 🔗 Live Demo
**https://3m5tf2ms.mule.page/**

---

## Hackathon Submission Q&A

### Why I built it & the core logic
MemeConsole was built to solve a simple problem: meme coin traders need to flip between five or six tabs (DexScreener, a wallet explorer, Twitter, a charting tool, and a chat window with an LLM) just to evaluate one token. MemeConsole puts all of that in a single console-style screen — search a token, see live price/volume/liquidity data and a candlestick chart, then ask an AI analyst a direct question about that exact token with all of its on-chain stats already injected into the prompt.

The core logic is a simple pipeline:
1. **Fetch** — query the DexScreener API for a token by name, ticker, or contract address (works across Solana, Ethereum, BSC, Base, and other major chains).
2. **Render** — display price, market cap, FDV, liquidity, 24h/6h/1h/5m price deltas, buy/sell transaction counts, an ECharts candlestick chart with EMA-20, and a holder-distribution donut chart.
3. **Contextualize** — package the live token data (price, price changes across timeframes, market cap, volume, liquidity, FDV, pair age, and buy/sell counts) into a structured text context.
4. **Analyze** — send that context plus a user question (or a quick-action preset like Signal, Risks, Smart Money, Rug Check, Price Levels, Rate It) to Claude (`claude-sonnet-4-6`) via the Anthropic Messages API, with a system prompt instructing it to act as a direct, no-fluff crypto analyst.
5. **Signal** — the app keyword-scans Claude's response for bullish/bearish language and renders a colored BULLISH / BEARISH / NEUTRAL badge above the answer.

### Trading Agent — why the strategy works, signals, decision-making, risk management
MemeConsole is currently an **analysis and research console, not an autonomous trading agent** — it does not place trades. The "strategy" it supports is human-in-the-loop: a trader uses the tool to rapidly triage tokens, and final buy/sell decisions are made by the person, not the software.

- **Signals used:** All signals are on-chain/technical, pulled live from DexScreener — price action across 5m/1h/6h/24h windows, volume, liquidity depth, FDV vs. market cap (dilution risk), pair age (rug/honeypot risk proxy), buy vs. sell transaction count ratios (order-flow pressure), and holder concentration (whale/LP risk). There is no macro or social-sentiment data feed yet — the "Social Pulse" sentiment score in the UI is currently a placeholder/simulated value, not a real social listening integration.
- **Decision-making:** Claude receives the structured on-chain snapshot above and a specific question (e.g. "Is this a potential rug?" or "Is smart money accumulating?"). It reasons over that snapshot only — it has no memory of past tokens or past decisions — and returns a structured, classified (bullish/bearish/neutral) text response. The decision to act stays with the human trader.
- **Risk management:** Risk is currently signaled, not enforced. The Rug Check and Risks quick-actions surface red flags (thin liquidity, high FDV/MCap ratio, concentrated holders, sell-pressure spikes, very young pair age) for the human to weigh, but there is no automated position sizing, stop-loss, or kill-switch logic in the app.
- **Asset class:** Spot, on-chain meme coin pairs across any EVM/Solana DEX pair indexed by DexScreener. No futures, derivatives, or prediction-market integration yet.

### Development challenges & status
**Challenges and solutions:**
- *Single-file constraint* — keeping the entire app (HTML/CSS/JS) dependency-free and buildless for trivial static-host deployment, while still supporting a real-time chart. Solved by using ECharts via CDN and vanilla JS DOM updates rather than a framework.
- *Token data normalization* — DexScreener's response shape varies by chain/DEX, with optional fields. Solved with defensive fallbacks (`||` defaults, optional chaining) throughout the rendering and AI-context-building code.
- *Keeping the AI grounded* — avoiding generic LLM output disconnected from the actual token. Solved by always injecting a fresh, structured on-chain snapshot into the prompt right before the question, rather than relying on the model's general knowledge.
- *Browser-only Anthropic calls* — calling the Anthropic API directly from client-side JS requires the `anthropic-dangerous-direct-browser-calls` header and exposes the API key client-side, which is a known limitation flagged in this README.

**Completed:**
- Universal token search (DexScreener)
- Live price/volume/liquidity/FDV/market cap display
- Candlestick chart with EMA-20 + volume bars, multiple timeframes
- Holder distribution donut chart
- AI signal analysis with quick-action presets and bullish/bearish/neutral classification
- Watchlist (localStorage persistence)
- Discover (trending/gainers/new tokens)

**Missing / simulated placeholders:**
- Wallet tracker (UI present, real portfolio/PnL data not wired up)
- Live trades feed (currently simulated, not a real WebSocket feed)
- Social Pulse sentiment (placeholder score, no real social-listening API)
- Any actual order execution / autonomous trading

**Next steps:**
- Real wallet portfolio via Moralis or Zapper API
- WebSocket-based live price/trade feed
- Portfolio PnL tracking with entry-price history
- Copy-trade alerts and multi-wallet comparison
- Price alerts / notifications
- Move the Anthropic API key server-side behind a proxy before any public deployment with a real key

### Frameworks, models, and APIs used
- Vanilla HTML/CSS/JS (no build step, no framework)
- [ECharts 5.5](https://echarts.apache.org/) — candlestick and donut charts
- [DexScreener API](https://docs.dexscreener.com/) — live token data (free, no key required)
- [Anthropic Claude API](https://docs.anthropic.com/) — model `claude-sonnet-4-6`, for AI signal analysis

**Bitget AI tools used:** _None currently integrated (no Agent Hub, Playbook, MCP Server, Skill Hub, or US Stocks Data API in this build)._

### Experience with Bitget AI tools, suggestions, and views on Agentic Trading
_Not yet evaluated in this build — add your own notes here once you've tried Bitget's Agent Hub / Playbook / MCP Server / Skill Hub / US Stocks Data API._

### Submission links
> ⚠️ Fill in before submitting — these can't be generated for you:
- **GitHub repo / MuleRun / GetAgent Studio link:** https://3m5tf2ms.mule.page/
- **Live trading record or paper trading log (timestamp, pair, side, price, size, balance changes):** _add link — note: this app does not currently log trades; you'll need a separate paper-trading log if required_
- **Repost of the official Bitget campaign post:** _add link_
- **Your project post (must include #BitgetHackathon and tag @BitgetAI):** _add link_
- **Demo video (if login required for any part of the demo):** _add link_

---

# MemeConsole 🚀

**Universal Meme Coin Tracker** — real-time data, wallet tracking, and AI signal analysis for every meme coin across every chain.

Live demo: deploy `index.html` to any static host (GitHub Pages, Vercel, Netlify), or view it live at **https://3m5tf2ms.mule.page/**

---

## Features

- **Universal Search** — search any token by name, ticker, or contract address. Powered by DexScreener API. Covers Solana, Ethereum, BSC, Base, and all major chains.
- **Real-Time Data** — live price, 24h/6h/1h/5m price changes, volume, market cap, FDV, liquidity, pair age, buy/sell transaction counts.
- **Candlestick Chart** — ECharts candlestick with EMA-20 overlay and volume bars. Multiple timeframes (5m, 15m, 1h, 4h, 1d).
- **Wallet Tracker** — paste any EVM or Solana wallet address to track holdings and PnL.
- **AI Signal Analysis** — powered by Claude (claude-sonnet-4-6). Ask anything about a token with full on-chain context injected automatically. Quick-action buttons: Signal, Risks, Smart Money, Rug Check, Price Levels, Rate It.
- **Live Trades Feed** — simulated real-time buy/sell feed for selected token.
- **Smart Money Alerts** — on-chain derived alerts: whale moves, LP concentration, momentum signals.
- **Holder Distribution** — donut chart showing token concentration across holder tiers.
- **Social Pulse** — sentiment score and social signal summary.
- **Watchlist** — star any token, persisted to localStorage.
- **Discover** — trending, gainers, and new token grids.

---

## Stack

- Pure HTML/CSS/JS — zero build step, zero dependencies to install
- [ECharts 5.5](https://echarts.apache.org/) — candlestick and donut charts
- [DexScreener API](https://docs.dexscreener.com/) — real-time token data (free, no key required)
- [Anthropic Claude API](https://docs.anthropic.com/) — AI analysis (requires API key)

---

## Setup

### 1. Clone

```bash
git clone https://github.com/YOUR_USERNAME/memeconsole.git
cd memeconsole
```

### 2. Add your Anthropic API key

Open `index.html` and find the `askAI` function. Replace the fetch headers to include your key:

```js
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'YOUR_ANTHROPIC_API_KEY',
  'anthropic-version': '2023-06-01',
  'anthropic-dangerous-direct-browser-calls': 'true'
}
```

> ⚠️ For production, proxy the Anthropic API call through a backend to keep your key secret. Never expose API keys in client-side code in a public repo.

### 3. Deploy

**GitHub Pages:**
1. Push to GitHub
2. Go to repo Settings → Pages → Source: main branch / root
3. Your app is live at `https://YOUR_USERNAME.github.io/memeconsole`

**Vercel / Netlify:**
Just drag and drop the folder, or connect your GitHub repo.

---

## Project Structure

```
memeconsole/
├── index.html      # Entire app — HTML, CSS, JS in one file
└── README.md
```

---

## API Notes

### DexScreener
- Free, no API key required
- Rate limit: ~300 requests/min
- Docs: https://docs.dexscreener.com/api/reference

### Anthropic Claude
- Requires API key from https://console.anthropic.com
- Model used: `claude-sonnet-4-6`
- Direct browser calls require the `anthropic-dangerous-direct-browser-calls` header

---

## Roadmap

- [ ] Real wallet portfolio via Moralis / Zapper API
- [ ] WebSocket live price feed
- [ ] Portfolio PnL tracking with entry price history
- [ ] Copy trade alerts
- [ ] Multi-wallet comparison
- [ ] Price alerts / notifications

---

## License

MIT
