---
title: "11. Execution & TCA"
layout: default
nav_order: 12
---

# Execution & TCA
{: .no_toc }

*~8 min read*

**Background**

## Why it matters

Research interviews rarely ask you to derive an optimal trading schedule. They do ask whether the alpha survives contact with the market. Transaction-cost analysis (TCA) is the vocabulary for that question: what price you decided at, what price you got, and which piece of the gap was delay, spread, or impact. This page is the intro. It is not a live trading system, and it does not cover brokerage APIs.

## Core concepts

- **Implementation shortfall is the full gap.** Perold's idea: compare the paper portfolio, marked from the decision price, to what you actually earned. The gap is not "the commission." It includes delay (the price moved before you finished), the spread and temporary impact you paid to get filled, fees, and opportunity cost on the shares you never got done. A backtest that marks everyone at the next close ignores most of that stack. Naming the stack is the interview-level skill.
- **Spread, temporary impact, permanent impact.** The spread (and commission) is the toll for crossing. Temporary impact is the extra you pay because you demanded liquidity now; it fades. Permanent impact is the price change that stays because the market learned something from the order, or because you moved the equilibrium. You do not need a calibrated model to say which term a given excuse is pointing at.
- **Almgren–Chriss is the intuition, not a production algo.** Trade faster and you pay more impact but hold less price risk on the remaining shares. Trade slower and expected impact falls while the risk of the unfilled position grows. The efficient frontier of schedules is that tradeoff. Risk-averse execution finishes sooner. A research backtest usually does not simulate this schedule. It uses a cost penalty. Say which one you did.
- **Benchmarks answer different questions.** Arrival price (the mid or last price when you decided) measures shortfall against the decision. VWAP asks whether you beat the market's own volume-weighted path over the window. Close asks whether you matched the auction or the official close. A strategy marked to the close can look fine on VWAP and still have a terrible arrival-price shortfall if you were slow. Don't mix the benchmarks in one sentence.
- **A cost model inside a research backtest is a sensitivity, unless you earned it.** Flat bps, a spread from the quote, or a participation penalty that rises with order size over average daily volume are all legitimate if labeled. They are not a substitute for TCA on live fills. If the gross edge per trade is a few basis points, show the result under a harsher assumption, not only under zero.
- **Capacity is where the alpha dies.** The same signal at tiny size and at a large fraction of daily volume is two different strategies, because impact scales with participation. "It works at $10 million" does not imply "it works at $1 billion." For an intern research case, a participation cap plus a simple impact haircut is the honest version of capacity. A full market-impact calibration is a different job.
- **Paper fills do not include you.** Historical prices are the tape without your order. That approximation is reasonable when you are small versus volume. It is the backtest pitfall in [Backtest pitfalls](../07-backtest-pitfalls/) when you are not.

## Mental model

```
decision price  ->  delay while you wait or schedule
                ->  spread + temporary impact at the fill
                ->  permanent impact left in the price
                ->  shares you did not finish (opportunity)
                = implementation shortfall versus the paper trade
```

A research notebook that stops at "next close, no cost" has measured the paper trade. TCA starts when you ask how much of that paper you could have kept.

## Interview questions

1. **Your signal earns 8 bps per trade gross, before any cost, with a one-day hold. What is the TCA objection?**
   Answer: 8 bps is the same order of magnitude as a spread plus commission on many names, and impact is extra. The net can be zero or negative even when the gross IC looks real. Show a cost assumption that includes at least half-spread plus a size penalty, and say what participation you assumed. If the net dies, the gross result is a forecast diagnostic, not a strategy.

2. **In one sentence, what tradeoff does Almgren–Chriss formalize?**
   Answer: Faster execution pays more market impact and less timing risk on the shares still held; slower execution does the opposite. The schedule is a choice along that frontier, not a single "optimal" speed for every trader.

3. **You beat VWAP and the PM is still unhappy. How is that possible?**
   Answer: VWAP is a path benchmark over the window you traded. The PM may care about arrival price: the mid when the decision was made. You can match VWAP and still be far through the arrival price if the stock moved before you started or while you worked the order. Different benchmarks, different questions.

4. **Why can't you read capacity off the zero-cost Sharpe?**
   Answer: The zero-cost simulation assumes your orders don't change the price and that every share fills at the historical print. As assets grow, participation rises, impact rises, and fills get worse. Capacity is the size where net expected value, under an impact model you are willing to defend, is no longer worth the risk — not a multiple of the paper Sharpe.

## Watch

- ["10 Ways Backtests Lie" by Tucker Balch](https://www.youtube.com/watch?v=wQrQwuWQ1FI) — Quantopian, QuantCon NYC 2015. Several of the ten are execution: trading the close, ignoring impact, and taking size the tape did not have. This is the right level for a research interview.

## Further reading

- Almgren and Chriss, "Optimal Execution of Portfolio Transactions," *Journal of Risk* (2001). [Author PDF](https://quantitativebrokers.com/s/Optimal-Execution-of-Portfolio-Transaction-_-AlmgrenChriss-1999.pdf) and [journal page](https://doi.org/10.21314/JOR.2001.041).
- Perold, "The Implementation Shortfall: Paper versus Reality," *Journal of Portfolio Management* (1988). The paper that names the gap between the paper portfolio and the implemented one.
- [Trading Costs of Asset Pricing Anomalies](https://www.aqr.com/Insights/Research/Working-Paper/Trading-Costs-of-Asset-Pricing-Anomalies) — Frazzini, Israel, and Moskowitz, on AQR's site. What published anomalies look like after a real cost model.
- Novy-Marx and Velikov, "A Taxonomy of Anomalies and Their Trading Costs," *Review of Financial Studies* (2016). [DOI](https://doi.org/10.1093/rfs/hhv063).
