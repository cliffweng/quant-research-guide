---
title: "10. Risk models"
layout: default
nav_order: 11
---

# Risk models
{: .no_toc }

*~8 min read*

**Interview occasional**

## Why it matters

A risk model tells you what the book is exposed to and how large a move you should expect. It is not an alpha model. Interviews use it to catch a specific mistake: calling a portfolio "idiosyncratic" when most of its variance is still industry, market, or a style factor. You should be able to write the one-line decomposition and say what each piece means.

## Core concepts

- **Factor risk plus specific risk.** The standard linear model for asset returns is r ≈ Xf + e, where X holds exposures (industry dummies, beta, size, value, momentum), f holds factor returns, and e holds specific returns. Portfolio variance is then approximately:

  h′Vh = h′X F X′h + h′D h

  h is the vector of weights, F is the factor-covariance matrix, and D is the specific-variance matrix (often diagonal). The first term is common-factor risk. The second is specific risk. If you cannot point at which term dominates, you do not know why the book moves.

- **Exposures are not the same object as alpha scores.** A risk exposure to value can be a characteristic loading used only to estimate variance. An alpha score is a forecast of expected return. Using the risk model as your alpha model, or vice versa, confuses a covariance story with a forecast. Teams often use a richer risk model than the three factors they cite in the alpha write-up. That is normal. Disclose both.
- **Sample covariance is a bad risk model on its own.** With hundreds of names and a modest window, the sample covariance is noisy and often not a stable input to an optimizer. Structured factor models (the XFX′ + D form) and shrinkage (Ledoit–Wolf is the textbook version) exist because of that. An interview answer of "I used the sample covariance of 500 stocks over 60 days" should make you flinch.
- **"Market neutral" is a statement about one exposure.** Net market beta near zero can sit next to a large industry exposure or a large size exposure. Look at the vector X′h, not a single beta. If one industry is half the factor variance, the book is an industry bet that happens to be dollar neutral.
- **Specific risk is where concentration hides.** Equal industry weights can still put specific risk in one name. The diagonal (or block) specific term is how a max-name discussion shows up in the variance, not just in a weight cap. See [Portfolio construction](../09-portfolio-construction/).
- **Risk models are estimated, so they are stale and wrong in crises.** Factor volatilities jump, correlations move toward one, and specific risk is not independent across names when a real event hits a sector. A risk model is a baseline for sizing and for attributing P&L. It is not a guarantee that a 1-standard-deviation band contains the next month.
- **Attribution closes the loop.** After the fact, split P&L into factor P&L (exposures times realized factor returns) and specific P&L. If the money came from the factor piece, you ran a factor portfolio. If it came from specific P&L and the specific piece is a few names, you got lucky or concentrated. Either way you learned something the Sharpe did not say.

## Mental model

```
portfolio variance
    = common factor variance (exposures × factor cov × exposures)
    + specific variance (name-level, mostly)

P&L after the fact
    = factor P&L + specific P&L
```

The risk model is how you stop describing every up month as "alpha."

## Interview questions

1. **Write the decomposition of portfolio variance in a factor risk model and say what you learn if the first term is 90% of the total.**
   Answer: h′Vh ≈ h′X F X′h + h′D h. If the first term is most of the variance, the book's risk is common-factor exposure, not a pile of independent residuals. You are being paid (or hurt) for factor bets. That may be intentional. It is not a specific-risk alpha story.

2. **Why is the sample covariance of raw stock returns a poor thing to drop straight into an optimizer?**
   Answer: Too many parameters for the length of the sample, so weights chase noise and the matrix can be ill-conditioned. A factor structure or shrinkage cuts the effective number of parameters. Mention that the means are usually even noisier than the covariance if the question turns into full mean-variance.

3. **You hedged market beta to zero and the daily residual is still obviously one sector. What exposure did the hedge leave in X′h?**
   Answer: Industry (or another non-market factor). Market beta is one row of exposures. Sector weights can be large with a zero market beta. Add industry factors to the risk model or constrain sector active weights, then recompute factor variance.

4. **How can a risk model and an alpha model disagree without someone being "wrong"?**
   Answer: They answer different questions. The alpha model forecasts expected residual or total return. The risk model forecasts covariance. A name can have a positive alpha score and a large specific variance; the portfolio step decides whether the return per unit of that variance is worth it. Conflict is a sizing decision, not a data error.

5. **A calm-sample risk model says this book should move about 20 bps a day. It moves 150 bps on an event day. Is the model refuted?**
   Answer: Not by one day. Correlations and volatilities change in stress, so a model estimated in a calm window will understate event days. The operational response is a stress scenario or a higher risk estimate in the tails, not a claim that factor models "don't work" because one day broke the band.

## Watch

- [Quantopian Lecture Series: Risk Factor Exposure](https://www.youtube.com/watch?v=Ep8Y5JfQoRg) — Quantopian. The lecture title on YouTube is misspelled ("Expsosure"); it is their factor-risk-exposure session.
- [Quantopian Lecture Series: Fundamental Factor Models](https://www.youtube.com/watch?v=P16zDtf0CE0) — Quantopian. Same family of models, from the characteristic side. Pair with the exposure lecture.

## Further reading

- Ledoit and Wolf, "Honey, I Shrunk the Sample Covariance Matrix," *Journal of Portfolio Management* (2004). [PDF](http://www.ledoit.net/honey.pdf).
- MSCI's Barra risk-model handbooks are the industry reference for the XFX′ + D equity risk model used in production. Read them as documentation of that decomposition, not as a backtest.
- Grinold and Kahn, *Active Portfolio Management* — active risk and attribution next to the fundamental law.
