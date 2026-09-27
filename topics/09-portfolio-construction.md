---
title: "09. Portfolio construction"
layout: default
nav_order: 10
---

# Portfolio construction
{: .no_toc }

*~8 min read*

**Interview occasional**

## Why it matters

Alpha construction tells you which names you like. Portfolio construction is how much you hold, against what hedge, under what limits. Research-intern interviews rarely ask you to solve a mean-variance program by hand. They do ask why the book is dollar neutral but still a sector bet, and why the paper portfolio's IR never showed up in the implemented one.

## Core concepts

- **Weights are a separate decision from scores.** A rank is not a weight. You still choose gross exposure, net exposure, and how aggressively scores map into positions (linear in the z-score, equal weight in the top and bottom quantiles, and so on). Those choices change turnover and factor exposures even when the score is fixed.
- **Dollar neutral is not beta neutral is not factor neutral.** Dollar neutral means weights sum to about zero (long notional ≈ short notional). The market beta of that book can still be far from zero if the longs are high-beta and the shorts are low-beta. Beta neutral means a chosen beta (usually to the market) is hedged, often with index futures or by constraining estimated betas. Sector neutral means industry weights are matched or zeroed. Say which constraints you imposed. See [Factor models](../04-factor-models/).
- **Mean-variance is fragile in the place people want it to be precise.** Markowitz optimization wants expected returns and a covariance. Expected-return estimates are noisy enough that unconstrained optimizers concentrate on the noisiest names. In practice the covariance, the constraints, and a risk target do more work than the raw expected-return vector. Shrinkage and constraints are not a failure of sophistication. They are the point. See [Risk models](../10-risk-models/).
- **Risk targeting versus dollar targeting.** Fixed dollar gross looks stable and hides the fact that volatility regimes change. Targeting a volatility (or a tracking-error budget) rescales the book as risk changes. The scaler must use trailing, knowable volatility, or it is another lookahead.
- **Turnover is a constraint you feel in costs.** A higher IC with much higher turnover can lose to a duller signal once costs are on. Put a turnover penalty or a max-trade rule in the construction, and report one-way turnover: half the sum of absolute weight changes is the usual definition, so you don't double-count sells and buys.
- **Transfer coefficient, again.** Constraints (position caps, sector bands, a borrow list, a turnover limit) pull implemented weights away from ideal weights. The gap is why a research IC does not equal a live information ratio. See [Alpha construction](../05-alpha-construction/).
- **Concentration is a risk choice.** Equal-weighting the extreme decile spreads single-name risk. Letting weights run with the raw score, or with an optimizer, can put a large fraction of risk in a few names. That can be intended. It should be measured. A max-name or max-industry cap is the usual boring control.
- **Long-only is a different strategy.** Against a benchmark, you can only overweight and underweight. You cannot express the short side of a score fully. The active weights, the tracking error, and the IR versus the benchmark are the objects, not the long-short spread from the paper.

## Mental model

```
scores -> ideal weights (if unconstrained)
              |
              v
        constraints and hedges
        (net, beta, sector, name cap, turnover, borrow)
              |
              v
        implemented weights -> exposures and turnover you actually report
```

If the ideal book and the implemented book are very different, present the implemented one. That is the strategy.

## Interview questions

1. **Your longs and shorts have equal dollar notional, and the book still falls 8% in a down month while the market falls 10%. What failed?**
   Answer: Dollar neutrality. The longs likely have higher market beta than the shorts, so net beta is positive. Estimate beta, hedge with the index or constrain betas in the optimizer, and check sector weights. Don't describe the book as market neutral based on gross dollars alone.

2. **Why do unconstrained mean-variance optimizers produce ugly, concentrated weights when you feed them historical average returns?**
   Answer: Sample means are very noisy relative to the differences the optimizer treats as real. It levers estimation error. Shrink means toward a simpler prior, constrain weights, or skip mean-variance and map a robust score into capped weights. The covariance has the same disease at a smaller scale. See [Risk models](../10-risk-models/).

3. **You add a 10% name cap and the backtest Sharpe drops. Is the cap "destroying alpha"?**
   Answer: It is changing the strategy. Some of the drop is the transfer coefficient: you refused positions the signal wanted. Some of the original Sharpe may have been one or two names. Report both books, the resulting concentration, and whether the capped book's residual IR is something you would actually run.

4. **Define one-way turnover and why a two-way number twice as large confuses a cost model.**
   Answer: One-way turnover is typically 0.5 × Σ |Δw|, the fraction of the book traded. Two-way counts both the sell and the buy and is about twice as big for a long-short rebalance. A cost in bps times the wrong turnover doubles or halves the cost drag. State which one you multiply.

## Watch

- [Quantopian Lecture Series: Position Concentration Risk](https://www.youtube.com/watch?v=I1z7B2_FarQ) — Quantopian. How a few names can dominate a book that looks diversified on a count of positions.
- [Quantopian Summer Lecture: The Art of Not Following the Market](https://www.youtube.com/watch?v=Af0l3TQJ3h8) — Quantopian. Regression and beta hedging, which is the practical version of "dollar neutral was not enough."

## Further reading

- Markowitz, "Portfolio Selection," *Journal of Finance* (1952). [DOI](https://doi.org/10.2307/2975974).
- Grinold and Kahn, *Active Portfolio Management* — active weights, tracking error, and implementation.
- Ledoit and Wolf, "Honey, I Shrunk the Sample Covariance Matrix," *Journal of Portfolio Management* (2004). [PDF](http://www.ledoit.net/honey.pdf).
