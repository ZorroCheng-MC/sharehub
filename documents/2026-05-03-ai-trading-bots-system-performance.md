---
title: "AI Trading Bots — How the System Works and How We're Doing"
date: 2026-05-03
type: article
status: draft
tags: [article, investing, ai-tools]
summary: "A plain-English briefing for the trading team on our four AI-powered paper trading bots — what each one does, why we built it that way, and how performance looks after the first 10 days."
hero: images/ai-trading-bots-system-performance-hero.jpg
created: 2026-05-03
---

# AI Trading Bots — How the System Works and How We're Doing

![](/images/ai-trading-bots-system-performance-hero.jpg)

## The Big Picture

We've been running a **paper trading testbed** since April 20, 2026 — a fully automated system that places real stock and options trades on a US brokerage account, but with simulated money. No real capital is at risk. The goal: prove out our strategies before going live.

**Account snapshot (as of April 27–29, 2026):**

| | |
|---|---|
| Portfolio value | $100,797.68 |
| Started with | $100,000.00 |
| Gain so far | +$797.68 |
| Cash sitting idle | $64,235.19 |
| Deployed in positions | ~$36,562 |
| Account type | Paper trading (Alpaca) |

Ten days in, four bots are running continuously — buying, selling, and managing risk — without anyone sitting at a screen.

---

## The Four Bots

### 1. AI Stock Picker (LLM Agent)
**What it does:** Every hour during US market hours, this bot uses Claude AI (Anthropic's language model) to analyze the latest stock trades filed by US politicians on the Capitol Trades disclosure database. It picks a politician, reviews their recent moves, checks whether those moves align with current market momentum, and decides whether to copy the trade at $500 per position.

**The logic:** Politicians as a group have historically outperformed the market — they have access to information and networks that ordinary investors don't. The AI layer is our filter: it doesn't blindly copy every trade, only ones that pass a momentum and market context check.

**10-day performance:**
- Profit: **+$2,602.87**
- Return: **+12.3%** on deployed capital
- Trades placed: 40
- Stocks currently held: 29 names including AAPL, GOOGL, META, MSFT, NVR, GS, AMD, BA

**Standout winners:**
- **NVR** (homebuilder): Bought at $6,675, now $7,600 — up **+$924 (+13.8%)**
- **PTON** (Peloton): +$37 (+7.5%)
- **MU** (Micron): +$33 (+6.8%)
- **NEE** (NextEra Energy): +$31 (+6.8%)

**What's not working:** A few short positions — AMZN and AWK — have moved slightly against us. The bot occasionally shorts stocks when politicians are selling; those haven't paid off yet.

---

### 2. Politician Consensus Tracker (Copy Bot)
**What it does:** This bot looks for situations where **two or more politicians** make the same trade in the same week — buying or selling the same stock independently. When that "consensus signal" appears, it copies the trade at ~$500 per position.

**The logic:** One politician buying a stock could be personal preference or coincidence. Two independent politicians making the same move in the same week is a much stronger signal. We're betting on the crowd wisdom of people with above-average market insight.

**Active consensus signals right now:**
- **MA (Mastercard):** Michael McCaul + Shelley Moore Capito both bought this week
- **VEA (Vanguard international ETF):** Multiple politicians buying

**10-day performance:**
- Profit: **+$1,509.45**
- Return: **+6.4%** on deployed capital
- Trades placed: 22
- Stocks held: AJG, AMZN, ASML, BSX, MA, SAP, SPGI, VEA, VTI, and others

---

### 3. TSLA Trailing Stop (Momentum Bot)
**What it does:** This bot holds a TSLA long position and manages it with two rules:
- **Trailing stop:** If TSLA falls more than 10% from its highest point since we bought in, the bot automatically sells — cutting losses before they get bigger.
- **Ladder buys:** If TSLA drops 20% or 30% from our entry price, the bot buys more shares, averaging down.

**The logic:** Momentum trading — ride the trend, let the stop handle the downside. If TSLA keeps climbing, we hold. If it reverses hard, we exit. The ladder buys are a long-term conviction bet: if it drops that much, we want more.

**10-day performance:**
- Loss: **-$149.36**
- Return: **-3.8%** on position
- Holding: 10 shares, average entry $392, current price ~$377

TSLA has been under pressure since we bought in. The trailing stop hasn't triggered (we'd need a drop from the post-entry peak), so we're holding. This bot is designed to be patient.

---

### 4. TSLA Wheel Strategy (Options Income Bot)
**What it does:** This bot generates income from TSLA options using the "wheel" strategy:

1. **Sell a cash-secured put** at a strike price below the current market price. The buyer pays us premium upfront. If TSLA stays above the strike at expiry — we keep the premium, the option expires worthless, and we do it again.
2. **If TSLA drops below the strike** and we get assigned shares — we then sell covered calls on those shares to generate further income while we hold.

Think of it like this: we're the "insurance company" selling protection on TSLA. We collect the premium. We only pay out if things go badly wrong.

**Current positions:**
- Short 1x TSLA Put at $330 strike (expires May 8) — collected $150 premium, currently worth $62, **profit so far: +$88**

**10-day performance:**
- Net P&L: **-$140** (one earlier put expired at a small loss, offset partially by the current winner)
- Trades: 5 (multiple puts opened and closed)

The options income strategy is working on the active position. The small net loss reflects an earlier put that was closed before expiry.

---

### 5. Short Closer (Background Bot)
This bot runs quietly in the background. When any short equity position reaches **+$5 or more in profit**, it automatically closes the position — locking in the gain before it reverses. It also ensures closing orders are attributed to the same bot that opened the short (for accurate performance tracking).

---

## Performance Summary

| Bot | Strategy | P&L | Return |
|-----|----------|-----|--------|
| AI Stock Picker | Hourly AI + politician picks | **+$2,602** | +12.3% |
| Consensus Tracker | Multi-politician agreement | **+$1,509** | +6.4% |
| TSLA Trailing Stop | Momentum + ladder buys | **-$149** | -3.8% |
| TSLA Wheel | Options premium income | **-$140** | -16%* |
| **Combined active bots** | | **+$3,822** | |

*-16% is on the small amount of capital at risk in each option, not the full account.

**Portfolio total:** +$797 from starting $100,000 — modest in absolute terms because **64% of capital is still in cash**. The bots place small $500 positions; at this scale, most of the account is uninvested. As we validate more strategies, we'll deploy more capital.

---

## What We're Watching

**Green flags:**
- The AI picker's stock selection has been strong — NVR is a standout call that a human trader might have missed
- Politician consensus signals are producing consistent small wins
- The TSLA wheel is collecting premium effectively on the current position

**Yellow flags:**
- TSLA is underperforming — both TSLA positions are slightly in the red
- A handful of short positions (AMZN, AWK) have gone against us
- Capital efficiency: $64K sitting idle is not generating returns

---

## What's Next

This paper trading account is our **proving ground**. Once we're confident in a strategy's edge, we consider moving it to real capital.

The team's open question — covered in detail in the [IBKR vs Alpaca analysis](https://sharehub.zorro.hk/documents/2026-05-03-alpaca-vs-ibkr-migration-analysis.html) — is whether to expand into **futures, forex, crypto, or international stocks**, which would require moving to Interactive Brokers. For the current US stocks + TSLA options setup, we stay on Alpaca.

Feedback from the team on what's working and what you'd want to see differently is very welcome.

---

*Created: 2026-05-03*
