---
title: "14. Interview case patterns"
layout: default
nav_order: 15
---

# Interview case patterns
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

QR and research-intern loops often hand you a signal, a chart, or a one-paragraph strategy and ask you to work. They are not asking for a new theorem. They are asking whether you reach for the right objection in the right order: mechanism, data timestamp, factor exposure, costs, and how many tries. This page is the pattern list. The content lives in the earlier topics. Brainteaser books are a different drill and are not a substitute for this one.

## Core concepts

- **Pattern A — "Here is a signal. How do you test it?"** Restate the mechanism and the horizon. Define the as-of time, the universe, and the forward return that matches the hold. Neutralize factors you do not intend to take. Freeze a simple portfolio rule. Put a cost on turnover. Only then talk about a Sharpe. Jumping to a model class (or to "I'd use machine learning") before the timestamp is a miss.
- **Pattern B — "The backtest Sharpe is 2. Do you believe it?"** Do not say yes or no to the number. Ask for lag, universe construction, delistings, trial count, sample length, and costs. A Sharpe of 2 with a same-bar fill or with today's index members is not a Sharpe of 2. See [Backtest pitfalls](../07-backtest-pitfalls/).
- **Pattern C — "Is this alpha or a factor bet?"** Regress the book on market, size, value, momentum, and sectors, or show the hedged residual. If the intercept dies, you found a factor implementation. That can still be useful. Call it what it is. See [Factor models](../04-factor-models/).
- **Pattern D — the data trap.** Current constituents, restated earnings, a ticker join, null returns filled with zero, a scaler fit on the full sample. Narrate the direction of the bias (usually up) and the fix (as-of membership, knowledge date, delisting return, trailing or cross-sectional scale). See [Data hygiene](../02-data-hygiene-survivorship/).
- **Pattern E — "Combine these two signals."** Ask how correlated the scores are and how correlated the P&L is. Residualize. Don't average two copies of value and report a diversification benefit. Check that the combination's turnover didn't eat the gross improvement. See [Alpha construction](../05-alpha-construction/).
- **Pattern F — "It lost money last year."** Separate a broken mechanism, a regime your mechanism should not have liked, a cost or crowding change, and a factor you accidentally held. One down year does not refute a slow premium. One down year in the exact regime you predicted would be strong does. See [Presenting research](../13-presenting-research/).
- **How to speak the answer.** State assumptions out loud. Name the bias by its name. Say which statistic you want (IC path, net spread, intercept, deflated hurdle) and which data timestamp it requires. End with what would falsify the claim. Silence while you draw the timeline is better than a fast, unstructured list.

## Mental model

```
1. What would have to be true economically?
2. What was knowable at the decision time?
3. What factor bet is still in the weights?
4. What happens after a boring cost and a lag?
5. How many shots produced the number on the slide?
```

Run the list in that order. Candidates who start at step 5 with a formula, or at a library name, skip the part the interviewer can grade.

## Interview questions

1. **Case: a candidate shows a long-short Sharpe of 1.6 from 2012–2024 on "S&P 500 stocks," signal at the close, trade at the close, no costs, no factor regression. Walk through your response.**
   Answer: Universe: is membership as-of or current? Fill: lag off the same close. Costs: even liquid names don't trade for free; ask turnover. Factors: the 1.6 may be market or momentum. Trials: how many signals before this one. You can do all of that without seeing the code. The number is not interpretable until those are answered.

2. **Case: two features, each with a decent gross spread, correlation of scores about 0.7. The interviewer asks whether to allocate half the risk to each. What do you say?**
   Answer: Not by default. Correlation 0.7 means the second book is mostly the first. Check residual spread of one after neutralizing the other, correlation of their returns, and net-of-cost IR of the combination versus the better single signal. Half risk to each is a portfolio choice you make after that, not before.

3. **Case: "We filled missing returns with zero and dropped names without 10 years of data." Bias, and fix?**
   Answer: Both choices tilt the sample toward survivors and replace likely-negative delisting outcomes with zero or with absence. Bias is upward for a long book, and unpredictable but usually flattering for a long-short that dropped the left tail. Keep names that existed at the decision date, include a delisting return, and don't require them to survive the future window.

4. **Case: the interviewer says "t-stat is 2.3, so it's significant." You learn the 2.3 is the best of a large in-house library. Reply?**
   Answer: The 2.3 is the maximum of a search, so a single-test 5% cutoff does not apply. Ask for the number of trials or use a holdout that library did not see. For a claim of a new equity factor against the published literature, Harvey, Liu, and Zhu's point is that you want something closer to t > 3, not 2. See [Statistical significance](../08-statistical-significance-multiple-testing/).

5. **You have a real project on your résumé. What is the smallest set of facts that makes the discussion about research instead of about a class assignment?**
   Answer: The mechanism, the as-of rule, the lag, gross versus net, one factor control, and one honest limitation. Stack names and a leaderboard rank do not substitute. If you did not log the number of specs, say the result is exploratory. See [Presenting research](../13-presenting-research/).

## Watch

- ["10 Ways Backtests Lie" by Tucker Balch](https://www.youtube.com/watch?v=wQrQwuWQ1FI) — Quantopian, QuantCon NYC 2015. The checklist behind pattern B. If you can reproduce his ten failure modes in your own words, you can survive the usual chart.
- [Quantopian Lecture Series: p-Hacking and Multiple Comparisons Bias](https://www.youtube.com/watch?v=YiDfbYtgUPc) — Quantopian. The simulation behind pattern B's trial-count question and pattern A's "don't sort until it works."

## Further reading

- This guide, topics [02](../02-data-hygiene-survivorship/) through [08](../08-statistical-significance-multiple-testing/). The case is those pages applied in order.
- Harvey and Liu, "Backtesting." [Author PDF](https://people.duke.edu/~charvey/Research/Published_Papers/P120_Backtesting.PDF).
- Puzzle books (Timothy Crack, *Heard on the Street*; Xinfeng Zhou, *A Practical Guide to Quantitative Finance Interviews*) cover mental math and probability that some loops still ask. They do not cover the research case above.
