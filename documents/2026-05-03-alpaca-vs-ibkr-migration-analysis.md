---
title: "Alpaca vs IBKR — Migration Analysis for AI Trading Platform"
date: 2026-05-03
type: article
status: draft
tags: [article, investing, ai-tools, mcp]
summary: "A detailed research-backed comparison of Alpaca and Interactive Brokers across fees, asset availability, market data quality, order types, and AI integration landscape — to inform a migration decision for a multi-strategy AI trading platform run from Hong Kong."
hero: images/alpaca-vs-ibkr-migration-analysis-hero.jpg
created: 2026-05-03
---

# Alpaca vs IBKR — Migration Analysis for AI Trading Platform

![](/images/alpaca-vs-ibkr-migration-analysis-hero.jpg)

## Context

The current system is an Alpaca paper trading testbed running 24/7 on a dedicated Mac, with four automated bots and a Flask dashboard:

| Bot | Strategy |
|---|---|
| Trailing Stop Monitor | TSLA 10% trailing stop + ladder buys at −20%/−30% |
| Wheel Strategy Monitor | TSLA cash-secured puts (PUT stage) → covered calls (CALL stage) |
| LLM Decision Agent | Claude picks a politician from Capitol Trades, copies trades at $500/trade |
| Short Closer | Auto-closes profitable equity shorts when unrealised P&L ≥ $5 |

The goal evaluated here: migrate from Alpaca to Interactive Brokers (IBKR) to build a more sustainable, multi-strategy AI trading platform — one that can support different strategy types including stable income strategies, higher-frequency approaches, and AI-driven multi-source analysis. The account holder is HK-based and already has an IBKR account.

---

## Fees Comparison

### Execution Costs

| | Alpaca | IBKR (Pro — only plan available to HK residents; Lite is US-only) |
|---|---|---|
| US stocks/ETFs | **$0 commission** | $0.005/share, min $1.00/order, max 1% of trade value |
| US options | **$0/contract** | $0.65/contract (premium ≥ $0.10), min $1.00/order |
| Options (lower premium) | $0 | $0.50/contract ($0.05–$0.10 premium); $0.25/contract (<$0.05) |

For small share counts (50–100 shares), IBKR's $1 minimum bites on every fill. For options, the gap is significant: a wheel strategy selling 1 contract/trade pays $0 on Alpaca vs $1.65 on IBKR — that's ~$60/month at 1 trade/day before counting commissions on the buy-to-close.

### Market Data

| | Alpaca | IBKR |
|---|---|---|
| Free tier | IEX (~15% of market volume) | Cboe One + IEX (non-consolidated) |
| Full NBBO (consolidated) | $99/month (Algo Trader Plus) — waived if $30k+ deposited | ~$10/month — **waived if commissions ≥ $30/month** |
| Options real-time (OPRA) | Included in Algo Trader Plus | ~$4.50/month add-on |
| **Effective total for full data** | **$99/month** | **~$14.50/month (often $0)** |

This is the hidden cost that flips the comparison for data-intensive strategies. A bot doing real-time AI analysis needs true NBBO, not IEX. IBKR's data is both cheaper and higher quality.

### Other Fees

| | Alpaca | IBKR |
|---|---|---|
| International wire | $50 | First withdrawal/month free |
| Inactivity fee | None | None (eliminated July 2021) |
| Margin rate (<$100k USD) | Fed Funds + 1.0% (~5.3%) | BM + 1.5% (~5.8%) |
| Margin rate (>$1M) | — | BM + 0.5% (IBKR wins significantly) |

### Net Cost Verdict for Current Bots

For the existing strategy mix (US stocks + TSLA options), **Alpaca is cheaper on execution**. Zero commissions on options outweigh IBKR's data cost savings. This changes if you move to strategies requiring real NBBO data or multi-asset instruments.

---

## Asset Availability

| Asset Class | Alpaca (HK resident) | IBKR (HK resident) |
|---|---|---|
| US stocks / ETFs | ✅ | ✅ |
| US equity options | ✅ | ✅ |
| Crypto (live) | ❌ US states only | ✅ BTC, ETH, SOL, ADA, XRP, DOGE + more |
| Futures (equity, rates, commodities) | ❌ | ✅ 30+ global exchanges |
| Forex | ❌ | ✅ Interbank access |
| International stocks | ❌ US-listed only | ✅ 33 countries, 160+ markets |
| US Treasuries / bonds | ⚠️ T-bills/notes via market order only | ✅ Full fixed income (corporates, munis, international) |

Alpaca is a US equities/options sandbox. For HK residents, crypto live trading, futures, forex, and international stocks are all unavailable. Bonds exist but are limited to market orders on US government securities only — no secondary market, no corporates, no munis.

---

## Beyond Asset Availability — The Real "Blocking" Factors

Even staying within US stocks, ETFs, and options, Alpaca has ceilings that affect strategy ambition:

### 1. Market Data Quality

Alpaca's free tier is IEX — a single exchange covering roughly 15% of US equity volume. It is **not** the National Best Bid and Offer (NBBO), the consolidated quote from all exchanges. For AI systems doing real-time price analysis, price signals are incomplete. Strategies that depend on spread, momentum, or microstructure data will be working with degraded inputs.

IBKR provides true NBBO, Level 2 order book depth, tick-by-tick data, and historical tick data for backtesting — for ~$14.50/month total.

### 2. Order Type Sophistication

| Order type | Alpaca | IBKR |
|---|---|---|
| Market, limit, stop, trailing stop | ✅ | ✅ |
| Good-till-cancelled (GTC) | ✅ | ✅ |
| Bracket orders | ❌ | ✅ |
| One-Cancels-All (OCA) | ❌ | ✅ |
| VWAP / TWAP algo | ❌ | ✅ |
| Pegged / adaptive | ❌ | ✅ |
| Iceberg (hidden size) | ❌ | ✅ |

For strategies that need to minimise market impact (larger positions) or manage risk with bracket/OCA structures, Alpaca's order type set is a hard ceiling.

### 3. API Rate Limits

Alpaca's default is 200 requests/minute (~3.3/sec). For bots scanning many tickers, streaming multiple symbols, or running at higher frequency, this becomes a bottleneck. IBKR's TWS API allows 50 messages/second (3,000/min) — 15× higher throughput.

### 4. Execution Quality (PFOF)

Alpaca uses payment for order flow — broker-dealer market makers pay Alpaca for the right to execute your orders. Those market makers profit from the bid-ask spread. For small retail trades, the impact is minimal. For a bot doing volume, execution quality erodes real P&L invisibly. IBKR Pro routes to the best venue across exchanges (SmartRouting) without PFOF.

### 5. Platform Longevity

Alpaca is a VC-backed startup (Series B). IBKR is a publicly traded company ($20B+ market cap) that has operated institutional trading infrastructure since 1978. For a platform you intend to invest serious development time into, broker durability matters.

---

## "Blocking" — Decision Matrix by Strategy Type

| Strategy type | Alpaca sufficient? | Key constraint |
|---|---|---|
| US stocks/ETFs, slow (minutes–hours) | ✅ Yes | $0 commission advantage is real |
| US options (wheel, spreads) | ✅ Yes | $0/contract is material vs $0.65 on IBKR |
| US stocks/ETFs, higher frequency (seconds–minutes) | ⚠️ Marginal | Rate limits + IEX data degrade AI signal quality |
| Multi-ticker real-time AI analysis | ⚠️ Marginal | IEX-only data; 200 req/min cap |
| Futures, forex, crypto, international | ❌ Hard block | Simply not available for HK residents |
| US bonds (corporates, munis, secondary) | ❌ Hard block | Only US Treasuries via market order |

The "blocking" is not just asset availability — it's market data quality and order sophistication capping what AI strategies can actually do, even within US equities.

---

## IBKR MCP Server Landscape

A survey of 13+ MCP (Model Context Protocol) servers for IBKR was conducted to assess AI integration options. All are unofficial and alpha-quality.

### Summary Table

| Server | API used | cancel_order | Trailing stop | Options chain | Unattended auth |
|---|---|---|---|---|---|
| code-rabi/interactive-brokers-mcp | Client Portal REST | ❌ | ❌ | ❌ | ❌ (24h+2FA) |
| rcontesti/IB_MCP | Client Portal REST | ⚠️ Untested | ❌ | ⚠️ Untested | ❌ (24h+2FA) |
| **YoungMoneyInvestments/ibkr-mcp** | **TWS socket** | **✅** | **✅** | **✅ (with Greeks)** | **✅ (with ibeam)** |
| danielkristofik/mcp_claude_ibkr | TWS socket | ✅ | ❌ | ✅ | ⚠️ Human token required |
| MrRolie/mm-ibkr-mcp | TWS socket | ✅ | ❌ | ⚠️ | ⚠️ Telegram approval |

### Best Option: `YoungMoneyInvestments/ibkr-mcp`

```bash
pip install ibkr-mcp
```

Capabilities: `cancel_order`, `place_trailing_stop`, `get_option_chain` (Greeks), bracket orders, OCA, TWAP/VWAP algo orders, market scanners, VaR calculation.

**Confirmed gaps** (small PRs to fix):
- No `orderRef` / `client_order_id` equivalent — 4-line addition per order tool
- No GTC time-in-force — add `tif` param to `place_order` and `place_trailing_stop`

**Unattended auth**: Pair with [ibeam](https://github.com/Voyz/ibeam) (812 stars, actively maintained) — Docker container that automates IB Gateway headless login including 2FA. Standard solution in the IBKR algo community.

### Critical Insight: MCP ≠ Right Tool for Cron Bots

All current bots use this pattern:
```
cron → bash script → alpaca CLI → Alpaca REST API
```

There is no `ibkr` CLI equivalent. MCP servers are designed to be invoked by an AI agent as tools, not called from a shell script. For systematic cron-based bot execution, the correct tool is **`ib_insync`** — the mature Python library for direct TWS API access.

---

## Recommended Architecture (If Migrating)

```
Strategy bots (Python scripts)
    └── ib_insync (TWS API, direct socket)
            └── IB Gateway (headless Docker via ibeam)
                    └── IBKR

Claude (AI decision layer — 3-phase pattern, unchanged)
    └── Phase 1: Shell/Python gathers market + political data
    └── Phase 2: claude -p receives structured prompt → returns JSON decision
    └── Phase 3: Python parses JSON → places orders via ib_insync

Dashboard (Flask — keep as-is)
    └── Swap run_alpaca() calls → ib_insync queries
    └── Attribution: use order.orderRef field (IBKR's equivalent of client_order_id)
```

### What Can Be Reused from the Alpaca Testbed

| Component | Reusable? | Notes |
|---|---|---|
| 3-phase LLM decision architecture | ✅ Fully | Core pattern is broker-agnostic |
| Dashboard Flask app + frontend | ✅ Fully | Swap data source layer only |
| Strategy logic (% thresholds, stages) | ✅ Fully | Numbers don't change |
| Cron schedule pattern | ✅ Fully | Same timing, same market-hours check |
| `bot=name:uuid` attribution pattern | ✅ Via `orderRef` | Maps directly to IBKR's `order.orderRef` field |
| Bash scripts calling CLI | ❌ Rewrite needed | No IBKR CLI exists; bots become Python scripts |

Migration complexity estimate: **1–2 weeks** for the bot execution layer rewrite. Dashboard and AI logic are portable as-is.

---

## Verdict

| Scenario | Recommendation |
|---|---|
| Current strategies only (US stocks + TSLA options) | **Stay on Alpaca** — zero commissions win |
| Adding ETFs, slow US strategies | **Stay on Alpaca** — no meaningful gain from migrating |
| Needing real NBBO data for AI analysis | **IBKR** — $14.50/month vs $99/month, better quality |
| Adding futures, forex, crypto, international | **Must use IBKR** |
| Long-term platform for serious multi-strategy system | **IBKR** — platform durability, order sophistication, data depth |

The Alpaca testbed has served its purpose well. For a team building toward a production-grade, multi-strategy AI trading platform, IBKR is the right foundation — not because current strategies demand it, but because every ambitious direction you'll want to explore will require it.

---

## Open Questions for Team Discussion

1. **Asset scope**: Is the goal to stay within US equities/options, or expand to futures, forex, crypto, international markets?
2. **Frequency**: What is the target strategy frequency? Minutes = Alpaca fine; Seconds = IBKR needed.
3. **Options commission priority**: Is $0/contract a strategic priority for the wheel strategy? ($0.65/contract on IBKR is real cost at volume.)
4. **Data quality**: Does AI analysis require true NBBO, or is IEX sufficient for current signal generation?
5. **Live trading timeline**: When does paper trading graduate to live? IBKR's live account infrastructure is more battle-tested.
6. **Team ops**: Can the team manage a persistent IB Gateway Docker container? Alpaca requires no local process.

---

*Created: 2026-05-03*
