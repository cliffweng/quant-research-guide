---
title: "05. Alpha construction"
layout: default
nav_order: 6
---

# Alpha construction
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

A characteristic is not a strategy. Interviewers want the path from a column of data to a position: how you rank, what you neutralize, how long you hold, and how you know the signal decayed instead of the plot looking nice. This is the page that connects [Factor models](../04-factor-models/) to a portfolio you could actually rebalance.

## Core concepts

- **Score, then portfolio.** Typical path: clean the characteristic, rank or z-score it in the cross-section on each date, optionally demean within industry or against other factors, then map scores to weights. Doing the map with raw dollar values (unscaled earnings, unscaled market cap) lets the largest names dominate for a units reason, not an economic one.
- **Cross-sectional ranks are the default for equities.** A z-score using the day's mean and standard deviation is comparable across time only if you are careful about outliers. Ranks are cruder and more robust. Either way, fit the scaling on that date's universe, not on the whole panel including the future.
- **Neutralize what you do not intend to sell.** If the idea is "cheap versus peers," subtract the industry mean before you rank. If you skip that, a sector that is uniformly cheap becomes the whole long book. That is a factor bet. Say whether you wanted it.
- **Match horizon to the mechanism.** A filing-drift story measured with a one-year hold, or a one-year value score traded every day, mixes a slow signal into fast turnover. Pick a holding period, then define the forward return over that same horizon when you evaluate.
- **Information coefficient (IC) is a forecast diagnostic, not P&L.** The usual IC is the rank correlation, on a given date, between the signal and the subsequent return over the holding period. You get a time series of ICs. The mean says whether the sort works on average. The standard deviation of IC says how noisy it is. Mean/std of IC (sometimes called IC IR) is a stability summary. It ignores costs, constraints, and weighting.
- **Quantile spreads are closer to a portfolio.** Sort into buckets, go long the top and short the bottom, and plot the spread. Report turnover. A high IC with violent name-churn can be untradable. A modest IC with stable membership can be the better book.
- **Decay belongs on one chart.** Compute the same IC for forward returns over the next day, week, month, and quarter. A signal that only "works" at a horizon you would never hold is not a strategy. Overlapping evaluation windows need the caveat in [Exploratory analysis](../03-exploratory-analysis/).
- **Combining signals is a risk decision.** Equal-weighting z-scores treats every signal as equally informative and ignores correlation. If two scores are the same value factor, you double-counted one bet. Residualize one on the other, or risk-weight them, and look at the correlation of their P&L, not just the correlation of the raw features.
- **The fundamental law is a budget, not a promise.** Grinold's relation is roughly IR ≈ IC × √breadth, with breadth the number of independent bets per year. Clarke, de Silva, and Thorley multiply by a transfer coefficient (TC) between 0 and 1 for constraints: you do not get the paper portfolio. Correlated bets reduce breadth. Quoting IR = IC × √N with N equal to the number of stocks is a common interview miss.

## Mental model

```
characteristic at t
    -> lag so it was knowable
    -> cross-sectional score (rank or z)
    -> neutralize industry / unwanted factors
    -> weights
    -> forward return over the hold you actually use
    -> IC path and quantile spread, then costs
```

Alpha construction ends when you can point at a weight and say which information, as of which timestamp, put it there.

## Interview questions

1. **You have analyst-revision data and you z-score it using the mean and variance of the entire 2000–2024 panel. What did you leak, and what do you do instead?**
   Answer: The full-sample mean and variance include future revisions, so today's z-score depends on data you did not have. Standardize using only the cross-section on that date (or a trailing window that ends at t).

2. **IC is 0.03 and the long-short Sharpe looks excellent. How can both be true, and what do you ask next?**
   Answer: A small average rank correlation can still produce a smooth spread if breadth is high and the implementation is unconstrained — or the Sharpe can be a leverage/cost illusion, a factor exposure, or leakage. Ask for the IC time series, the quantile spread net of costs, factor loadings, turnover, and whether the evaluation returns overlap.

3. **Two signals each have IC 0.04 and correlation 0.8. What do you expect from combining them?**
   Answer: Much less than double the information. The second score is mostly the first. The combined book should be close to the better single book after you account for any reduction in noise. Check residual IC of one after neutralizing the other, and the correlation of their returns.

4. **Explain transfer coefficient in one sentence, and why constraints lower it.**
   Answer: TC measures how closely implemented weights match the ideal weights from the signal. Position limits, sector caps, turnover caps, and a no-short list all push weights away from the ideal, so realized IR is IC × √breadth × TC, not the unconstrained figure.

5. **Your value score flips sign every week and the one-week IC is strong, but one-month IC is flat. Is that a value strategy?**
   Answer: No. Classic value is a slow characteristic. A weekly sign flip is a different, high-turnover process, and the one-week IC may be microstructure or a bug. Hold it for a horizon that matches the mechanism, and put a cost model on the turnover before comparing it to a slow value book.

## Watch

- [Quantopian Lecture Series: Factor Analysis](https://www.youtube.com/watch?v=v5IYcBxMDYE) — Quantopian. Alphalens-style factor tears: quantiles, returns, and turnover.
- [Quantopian Summer Lecture Series: The Good, The Bad, and The Correlated](https://www.youtube.com/watch?v=GM76JkrVmRk) — Quantopian. Correlation and rank correlation, which is the machinery under IC.

## Further reading

- [alphalens](https://github.com/quantopian/alphalens) — the open-source factor tear sheet the Quantopian lecture demonstrates. Useful as a checklist of plots, not as a mandate to use that stack.
- Grinold, "The Fundamental Law of Active Management," *Journal of Portfolio Management* (1989), and the constraints extension by Clarke, de Silva, and Thorley, *Financial Analysts Journal* (2002).
- Grinold and Kahn, *Active Portfolio Management* — IC, breadth, and implementation in one place.
