# Limnal Research project overview

**Daily macro-regime monitoring for active investors.** Live at [limnalresearch.com](https://limnalresearch.com), by subscription.

> This repo describes what Limnal Research does and how it's built. The source code is private because it's a paid product.

## What it does

Six independent models feed one allocation function. The function gives a single measure of current market risk, and subscribers read it in a dashboard.

The six models cover macro regime, expected return, trend, credit, futures positioning, and dealer flows. The data comes from FRED, the U.S. Treasury, CFTC, options markets, and equity providers.

## How it's built

```
 Data sources (FRED, U.S. Treasury, CFTC, options, equities)
          │
          ▼
 20 scheduled jobs  ──(GitHub Actions cron)──▶  FastAPI service (Railway)
          │                                           │
          ▼                                           ▼
     Postgres  ◄──────── six models ───────▶  allocation function
          │
          ▼
 Next.js app on Supabase  ──▶  public site + subscriber dashboards
```

A historical-replay harness backtests the models. It reports performance, drawdown, Sharpe, Calmar, VaR, and Expected Shortfall across rebalance cadences and benchmarks.

## A problem I fixed

Model runs started going missing with no error. I traced it to the scheduler dying whenever the server scaled to zero. I moved scheduling out of the server into GitHub Actions cron. I also added an idempotent catch-up job that finds gaps and backfills them without creating duplicates.

I'm now building an agent that checks all model outputs and calculated values for consistency.

## How I work on it

I build with Claude Code. I write the requirements and review the output until it holds up.

---
[Profile](https://github.com/AakashAmorce) · [aakashdesai.com](https://aakashdesai.com)
