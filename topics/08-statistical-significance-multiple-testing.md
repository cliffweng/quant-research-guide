---
title: "08. Statistical significance & multiple testing"
layout: default
nav_order: 9
---

# Statistical significance & multiple testing
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

Finance research runs thousands of tests and publishes the winners. A t-stat of 2.0 on the one specification you kept is not the same object as a t-stat of 2.0 on the only specification you wrote down. QR interviews come back to this constantly, because it is the difference between a researcher and someone who sorted until the cell turned green.

## Core concepts

- **A t-stat is a standardized mean, not a blessing.** For roughly independent excess returns, the t-stat of the mean is about (per-period Sharpe) × √T, where T is the number of periods. Two traps: using the *annualized* Sharpe in that product double-counts the √12 or √252, and ignoring autocorrelation overstates T. Lo (2002) is the usual reference for Sharpe ratios when returns are dependent.
- **Significance is about the procedure, not the cell you highlight.** If you ran one predeclared test, conventional levels mean what the textbook says. If you ran K tests, or you peeked and stopped when it "worked," the probability that the best one looks good under the null is much higher than 5%.
- **Bonferroni versus false discovery rate.** Bonferroni (and its close relatives) controls the chance of *any* false positive in the family. Divide the significance level by K, or multiply p-values by K. It is simple and harsh, especially when tests are correlated (trying 50 value metrics is not 50 independent bets). Benjamini–Hochberg controls the expected *fraction* of rejected hypotheses that are false. That is usually the more relevant scientific target when you are screening many candidate signals and can tolerate some false leads that later die in a holdout.
- **Correlation among tests cuts both ways.** Naive Bonferroni over-penalizes a family of near-duplicate specs. Ignoring multiplicity because "they were related" under-penalizes. Methods exist that take dependence into account (White's Reality Check is the classic bootstrap for "is the best strategy in this set real?"). You are not expected to derive the bootstrap in an intern loop. You are expected to refuse a raw p-value on the winner.
- **The Harvey–Liu–Zhu hurdle.** Harvey, Liu, and Zhu document the factory of published equity factors and argue that a newly proposed factor, given that history, needs a t-stat above 3.0 rather than 2.0. That is a field-level multiple-testing adjustment, not a law that every internal diagnostic must clear. A single pre-registered test inside a firm can use a lower bar. A search over a vendor factor zoo cannot quote t = 2.1 as confirmation.
- **Deflated and haircut Sharpes.** Bailey and López de Prado's deflated Sharpe raises the bar for the *selected* Sharpe using the number of trials, the spread of Sharpes across trials, sample length, and non-normality (skew and kurtosis). Harvey and Liu's haircut turns a Sharpe into a t-stat, adjusts the p-value for multiple testing, and turns it back into a lower Sharpe. Same idea: the number you publish should be harder to achieve when you tried more things.
- **Holdouts do not replace counting.** A final untouched test set is the cleanest correction when you actually leave it untouched. It does not absolve a research program that burned twenty holdouts over five years. Write the test down, or keep a trial log. The trial log is the input to every deflation method above.
- **Economic size still matters.** A t-stat of 4 on a 2 bp effect with 80% annual turnover is a precise way to lose money after costs. Significance without a net-of-cost magnitude is not a strategy.

## Mental model

```
one predeclared test     ->  textbook p-value is about that test
best of K tests          ->  p-value must be about the maximum, not the winner's marginal test
best of a literature     ->  the hurdle moves up as the literature grows (HLZ: t > 3 for a new factor)
```

The question to answer out loud: "How many shots did this result take?"

## Interview questions

1. **You have 10 years of monthly returns, an annualized Sharpe of 1.0, and someone says the t-stat is 1.0 × √120. What is wrong?**
   Answer: The √T formula wants the per-period Sharpe, not the annualized one. Monthly Sharpe is about 1.0 / √12, and T is 120 months, so the t-stat is on the order of (1/√12) × √120 ≈ √10 ≈ 3.2 if months were independent — not √120. Autocorrelation would pull that down. Quote the frequency you used.

2. **You tested 20 independent signals and the best p-value is 0.04. Do you reject the null at 5%?**
   Answer: Not for the family. Under a Bonferroni bar the cutoff is 0.05/20 = 0.0025. A minimum p of 0.04 is ordinary when you take the best of 20 nulls. Benjamini–Hochberg is less harsh but still will not treat 0.04 on the winner as a single-test 5% result. Check a holdout or deflate the statistic.

3. **Why do people cite a t-stat of 3 for a new cross-sectional factor?**
   Answer: Harvey, Liu, and Zhu argue that hundreds of factors have already been tried in the published literature (and more were tried and not published), so the old t > 2 cutoff does not control false discoveries for a *new* claimed factor. Their estimate of an appropriate hurdle is a t-stat greater than 3. It is a multiple-testing statement about the literature, not a replacement for a clean experiment.

4. **Your tests are 20 highly correlated variants of one value signal. Why is Bonferroni a poor description, and what do you do instead?**
   Answer: The effective number of independent tests is closer to 1 than to 20, so Bonferroni will call almost everything insignificant, including a real value premium. Don't flip to "so we ignore multiple testing." Report the cluster as one idea, show the variants as robustness, and confirm the predeclared version out of sample. A dependence-aware method (or a single holdout) matches the design better than K = 20 independent shots.

5. **What inputs does a deflated Sharpe need that an ordinary Sharpe does not?**
   Answer: At least the number of trials (or an estimate of how many independent backtests you ran), something about how dispersed those trial Sharpes were, the sample length, and a treatment of non-normality. Without a trial count, there is nothing to deflate. The ordinary Sharpe assumes you computed one strategy, once.

## Watch

- [Quantopian Lecture Series: p-Hacking and Multiple Comparisons Bias](https://www.youtube.com/watch?v=YiDfbYtgUPc) — Quantopian. A concrete simulation of how more tests manufacture significant p-values, plus Bonferroni and holdouts.
- [Dangers of Backtest Overfitting](https://www.youtube.com/watch?v=QxhxLwNbMMg) — Marcos López de Prado. Multiple testing across backtests and the idea of a deflated Sharpe hurdle.

## Further reading

- Harvey, Liu, and Zhu, "… and the Cross-Section of Expected Returns." [NBER w20592](https://www.nber.org/papers/w20592).
- Harvey and Liu, "Backtesting." [Author PDF](https://people.duke.edu/~charvey/Research/Published_Papers/P120_Backtesting.PDF).
- Benjamini and Hochberg, "Controlling the False Discovery Rate," *Journal of the Royal Statistical Society, Series B* (1995). [DOI](https://doi.org/10.1111/j.2517-6161.1995.tb02031.x).
- White, "A Reality Check for Data Snooping," *Econometrica* (2000). [DOI](https://doi.org/10.1111/1468-0262.00152).
- Bailey and López de Prado, "The Deflated Sharpe Ratio." [SSRN 2460551](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2460551).
