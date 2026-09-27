---
title: "04. Factor models"
layout: default
nav_order: 5
---

# Factor models
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

"Alpha" in a research interview almost always means return that is left after a factor model, not return that is above zero. If your long-short book is just price momentum with extra steps, the interviewer will see it in a regression you did not run. Factor models are also the language of risk: what you meant to bet, and what you accidentally bet.

## Core concepts

- **A factor is a portfolio (or a repeatable sort), not a vibe.** The market, size, value, and momentum factors are excess-return series built from rules. A characteristic (book-to-price, 12-minus-1-month return) is the input to that portfolio. Don't confuse the column in your dataframe with the factor return.
- **CAPM is the one-factor case.** Regress excess returns on excess market returns. The intercept is CAPM alpha. The slope is beta. A strategy can make a lot of money and still have an intercept of zero if it was mostly long the market.
- **Fama–French and Carhart are the usual equity baselines.** The three-factor model adds size (SMB) and value (HML) to the market. The five-factor model adds profitability (RMW) and investment (CMA). Carhart adds momentum (often WML or UMD) to the three-factor model. You are not expected to recite every sorting breakpoint. You are expected to know what each factor is trying to capture and to use the published series honestly. Ken French's library documents the construction.
- **Time-series alpha versus cross-sectional premia.** A time-series regression of your *portfolio* on factor portfolios gives an intercept: average return not explained by those factors. A cross-sectional regression (Fama–MacBeth is the classic procedure) asks whether a characteristic predicts *next-period* differences across stocks. Same word "factor," different regression. Say which one you ran.
- **Residual is the claim.** If you sell the strategy as market- and sector-neutral, show the regression (or the hedged portfolio), not the gross chart. A significant raw return with a large HML loading is a value backtest. That can still be a good portfolio. It is not a new alpha until you say so.
- **Three families, don't mix them up.** Fundamental or characteristic factors (value, quality, industry). Macro factors (growth, inflation, rates). Statistical factors (principal components of returns). Characteristic models explain *why you hold the name*. Statistical factors often explain *variance* with factors you cannot narrate. Risk models in [Risk models](../10-risk-models/) lean on the covariance; alpha work leans on the expected-return story.
- **Loadings move.** A 60-month beta is an estimate, and it is unstable. Hedging with a stale beta leaves residual market risk. See the instability point in [Exploratory analysis](../03-exploratory-analysis/).
- **Industry is a factor even when you didn't ask.** Many "anomalies" shrink once you demean within industry. If your mechanism is not an industry bet, neutralize industry before you celebrate.

## Mental model

```
portfolio excess return_t
    = alpha
    + beta_mkt * MKT_t
    + beta_value * HML_t
    + ... 
    + residual_t

alpha is what you still claim after the terms you wrote down
residual is what the model did not catch — including misspecification
```

If you cannot list the terms on the right-hand side, you do not yet know what the backtest was.

## Interview questions

1. **Your long-short equity book has a Sharpe of 1.2 with no factor regression. What is the interviewer's objection?**
   Answer: The 1.2 may be compensation for market, size, value, momentum, or sector exposure. Regress the book's excess returns on a predeclared factor set (at least market, and usually SMB, HML, and momentum) and talk about the intercept, the loadings, and the residual volatility. Hedge or neutralize exposures you did not mean to take.

2. **What is the difference between a stock's book-to-price and the HML factor?**
   Answer: Book-to-price is a characteristic of a stock at a date. HML is a portfolio that is long high book-to-price names and short low ones, under a specific construction (Ken French's published series is the usual reference). Using the characteristic as a signal is alpha construction. Using HML as a regressor asks whether your portfolio was just that bet.

3. **You neutral market beta and still lose money whenever oil falls. What did the one-factor hedge miss?**
   Answer: Beta to the market is not beta to every macro or industry shock. The book can be sector-concentrated (energy) with a market beta near zero. Add an industry or commodity factor, or constrain sector weights. "Market neutral" is not "factor neutral."

4. **When would you use a statistical factor model instead of Fama–French?**
   Answer: When the goal is to describe covariance — hedging, risk, residual variance — and you do not need each factor to have a fundamental name. For an alpha claim, a statistical residual is harder to defend because you cannot say what economic bet you removed. Many shops use fundamental factors for the story and a statistical or fundamental risk model for the variance. See [Risk models](../10-risk-models/).

5. **Fama–MacBeth versus a single time-series regression of your backtest: which one answers "does the characteristic predict relative returns"?**
   Answer: The cross-sectional approach (Fama–MacBeth): each period, regress next returns across stocks on the characteristic, then average those slopes over time. The time-series regression of one portfolio on factor portfolios answers "did this portfolio have an intercept," which depends on how you built the portfolio.

## Watch

- [Quantopian Lecture Series: Fundamental Factor Models](https://www.youtube.com/watch?v=P16zDtf0CE0) — Quantopian. Characteristic factors, from the sort through the model.
- [Cliff Asness on Factor Investing and the History of Financial Economics](https://www.youtube.com/watch?v=2QrPCewZO9E) — Hoover Institution. Long interview (about 79 minutes) on value, momentum, and how factor investing actually got built. Watch the factor sections if you are short on time.

## Further reading

- Fama and French, "Common risk factors in the returns on stocks and bonds," *Journal of Financial Economics* (1993). [DOI](https://doi.org/10.1016/0304-405X(93)90023-5).
- Fama and French, "A five-factor asset pricing model," *Journal of Financial Economics* (2015). [DOI](https://doi.org/10.1016/j.jfineco.2014.10.010).
- [Ken French data library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html) — the portfolios and the construction notes.
- [Value and Momentum Everywhere](https://www.aqr.com/Insights/Research/Journal-Article/Value-and-Momentum-Everywhere) — Asness, Moskowitz, and Pedersen. Same premia across markets, which is the usual counter to "it's just a US equity artifact."
