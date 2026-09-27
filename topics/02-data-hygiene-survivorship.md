---
title: "02. Data hygiene & survivorship"
layout: default
nav_order: 3
---

# Data hygiene & survivorship
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

Most fake alpha is a data bug with a Sharpe ratio. Interviewers will hand you a strong backtest and ask whether the universe, the fundamentals, or the returns could have been known at the time. If you cannot explain survivorship, delistings, and point-in-time fundamentals, the rest of the interview is theater.

## Core concepts

- **Survivorship bias** keeps the names that still exist (or that are in the index today) and drops the ones that died, merged, or were kicked out. Failures are disproportionately negative. A backtest on today's S&P 500 members over 20 years is not a test of "the S&P 500." It is a test of stocks that ended up in the index, including winners that were added later.
- **Use membership as of the decision date.** Index adds and deletes are information. Joining "was this a member?" on the *current* constituent list leaks the future. The same bug shows up with "stocks that have 10 years of history": requiring a full future path drops names that delist halfway through.
- **Delisting returns are the painful rows.** When a stock leaves the tape, the last traded price is often not the economic exit, especially in bankruptcies. CRSP provides a delisting return. Dropping those rows, or filling a missing return with zero, pushes results up. Shumway (1997), "The Delisting Bias in CRSP Data," *Journal of Finance*, is the classic write-up of that bias.
- **Point-in-time versus restated.** Compustat-style fundamentals get restated. A 2014 earnings figure you download today may not be the figure an investor could have used in 2014. Fiscal period-end is also not the announcement date: the 10-K lands later. A usable panel is keyed by *knowledge date*, with a reporting lag you can defend.
- **Corporate actions.** Splits and dividends have to be applied consistently. Mixing split-adjusted prices with unadjusted shares, or using price return where the claim is total return, invents performance. Total return is the default for strategy research unless you are explicitly studying price-only effects.
- **Identifiers rot.** Tickers get reused. Map through a stable id (PERMNO/PERMCO in CRSP, or the vendor's security id) and an as-of date. A ticker join is a silent merge with the wrong company.
- **Missingness is informative.** Small, distressed, and newly listed names are missing more often. Dropping "incomplete" rows changes the universe toward survivors. Document the filter in the same sentence as the result.
- **Vendor convenience is not a research design.** A prebuilt "clean" equity file that backfills index membership or uses latest fundamentals will look easier and more profitable. Ask what was known on the trade date.

## Mental model

```
decision date t
    |
    +-- universe: members as of t, not as of today
    +-- fundamentals: values with a filing/knowledge date <= t
    +-- prices and shares: corporate-action convention stated
    +-- exit: include delisting return if the name dies after t
    |
    v
only then compute the signal and the next period's return
```

If any input to the signal is timestamped after t, the backtest is allowed to see the future. Treat that as a failed run, not a sensitivity.

## Interview questions

1. **Your friend's backtest trades the current S&P 500 constituents from 2000 to today and shows a high Sharpe. What is the first bias you name?**
   Answer: Survivorship / look-ahead in the universe. Today's members include firms that were not in the index in 2000, and exclude firms that were removed after poor performance. Rebuild membership as of each rebalance date, and keep delisting returns for names that leave the tape.

2. **You use book-to-price as a signal on fiscal year-end. Why can that still be lookahead?**
   Answer: Book equity is not public on the fiscal period-end date, and the number in a current database may be a later restatement. Lag the signal until the filing (or a conservative reporting lag) and use a point-in-time snapshot if you have one.

3. **A small-cap strategy's backtest has no rows for several bankruptcies because the return field was null. The author filled nulls with 0. Direction of the bias?**
   Answer: Upward. Delisted, distressed names are missing precisely when the economic return is often very negative. Filling with 0, or dropping them, truncates the left tail. Use a delisting return, and say so.

4. **Why is joining two vendor tables on ticker and date unsafe?**
   Answer: Tickers are reused and change. The join can attach fundamentals or returns from a different security that later inherited the ticker. Join on a permanent security identifier and validate coverage after the merge.

5. **What is the difference between a split-adjusted price return and the total return you usually want?**
   Answer: Split adjustment fixes the price level so returns don't jump on a split, but it does not add dividends. A dividend-paying stock's price return understates the investor's gain. Strategy P&L should use total return unless the hypothesis is specifically about prices.

## Watch

- [Quantopian Lecture Series: Universe Selection](https://www.youtube.com/watch?v=oa5RhuHVbH0) — Quantopian. How the set of names you are allowed to trade changes the result.
- ["10 Ways Backtests Lie" by Tucker Balch](https://www.youtube.com/watch?v=wQrQwuWQ1FI) — Quantopian, QuantCon NYC 2015. Survivor bias is one of the ten; the rest belong with [Backtest pitfalls](../07-backtest-pitfalls/).

## Further reading

- Brown, Goetzmann, Ibbotson, and Ross, "Survivorship Bias in Performance Studies," *Review of Financial Studies* (1992). [DOI](https://doi.org/10.1093/rfs/5.4.553).
- [Ken French data library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html) — factor returns plus the methodology notes that show how a careful universe is actually built.
- CRSP equity data is the usual source for delisting returns and historical identifiers; it is distributed through [WRDS](https://wrds-www.wharton.upenn.edu/).
