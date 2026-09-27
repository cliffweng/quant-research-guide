---
title: "03. Exploratory analysis"
layout: default
nav_order: 4
---

# Exploratory analysis
{: .no_toc }

*~8 min read*

**Interview occasional**

## Why it matters

Exploratory analysis is how you find broken joins, insane outliers, and a signal that is secretly a sector dummy. It is not how you declare that a strategy works. Interviewers use a short EDA discussion to see whether you look at the data before you fit it, and whether you know that every plot you "liked" is a peek you may have to pay for later. See [Multiple testing](../08-statistical-significance-multiple-testing/).

## Core concepts

- **Plot the raw material before the portfolio.** Coverage by date, fraction of names with a finite score, histogram of the signal, and a few extreme rows. A signal that is missing for half the universe in 2009 is a data story, not an alpha story.
- **Returns are heavy-tailed.** Means and Sharpe ratios move when a handful of days move. Report a robust picture (median, winsorized mean, or a simple trimmed spread) next to the raw one, and say which you will trade. Winsorizing a *signal* to limit outlier weights is a portfolio choice. Winsorizing *returns* until the backtest looks good is editing the evidence.
- **Cross-section and time series answer different questions.** A single day's cross-sectional rank correlation says whether the sort worked that day. The time series of those correlations says whether it works as a process. One scatter of all stock-months pooled together mixes the two and flatters dependence.
- **Overlapping windows fake precision.** A 12-month return updated every month shares 11 months with the next point. The cloud looks tighter than the number of independent bets. For exploration, also look at non-overlapping slices. For inference, see [Statistical significance](../08-statistical-significance-multiple-testing/).
- **Correlation is not a position.** Two signals can be highly correlated and still differ in tails, in costs, or in which industry they load on. Check rank correlation, but also check industry means and the top and bottom names.
- **Estimates move.** A beta, a mean, or an IC estimated on the first half of the sample is a random variable. If the second half does not resemble it, you do not have a stable input. That is a reason to simplify, not a reason to average windows until the picture calms down.
- **Separate bug-hunting from hypothesis search.** A reversed sign on a split, a duplicated date, or a return of 10,000% is a bug. A pattern you noticed in residuals and now want to trade is a new hypothesis. Label it as exploration and hold it out. See [Research question](../01-research-question-hypothesis/).

## Mental model

```
[ coverage and outliers ] -> [ distribution of the signal ] -> [ one-day cross-section ]
            |                                                          |
            v                                                          v
     fix joins / units                                      [ time series of that stat ]
                                                                       |
                                                                       v
                                                          stop, or write a new test
```

EDA is a filter for garbage and a source of questions. The question still has to survive a test you did not use to invent it.

## Interview questions

1. **You compute a mean pairwise correlation of 0.9 between two features and conclude they are the same alpha. What else do you look at?**
   Answer: Rank correlation is a start, but check whether they pick the same tails, the same industries, and the same names after you neutralize sector. Also check turnover and missingness. High correlation does not mean the tradable spread, or the cost, is the same.

2. **Why can a scatter of overlapping 12-month returns look more significant than it is?**
   Answer: Adjacent points reuse almost the same window, so the effective sample size is much closer to the number of non-overlapping years than to the number of months plotted. The picture understates uncertainty. Confirm with non-overlapping points or a dependence-aware standard error later.

3. **A stock shows a one-day return of several thousand percent. What are the likely causes, and what do you not do?**
   Answer: Likely a bad join, a split not applied, or a decimal error. Check the corporate-action convention and the identifier. Do not winsorize the whole return panel until the Sharpe looks reasonable and move on. Fix the row or document a precommitted winsor rule that you apply the same way out of sample.

4. **How do you keep EDA from becoming a silent multiple-testing machine?**
   Answer: Decide in advance which plots are bug checks (coverage, units, duplicates) and which are model search. Anything you choose because it looked good — a cutoff, a sector, a lag — gets counted as a trial and confirmed on data that did not generate it. See [Backtest design](../06-backtest-design/).

## Watch

- [Quantopian Lecture Series: Plotting Data](https://www.youtube.com/watch?v=nKq_wz3Qk8w) — Quantopian. A short, practical pass on looking at series before modeling them.
- [Quantopian Lecture Series: Instability of Parameters](https://www.youtube.com/watch?v=2pbu3_6lF40) — Quantopian. Why a mean or similar estimate from one window is a weak thing to trust.

## Further reading

- Lo, "The Statistics of Sharpe Ratios," *Financial Analysts Journal* (2002). [DOI](https://doi.org/10.2469/faj.v58.n4.2453). Autocorrelation changes how precise a performance number is — relevant as soon as your EDA starts quoting Sharpes.
- [Quantopian Lecture Series: The Good, The Bad, and The Correlated](https://www.youtube.com/watch?v=GM76JkrVmRk) — correlation as a tool, including the ways it misleads. Listed here as a video essay, not a paper.
