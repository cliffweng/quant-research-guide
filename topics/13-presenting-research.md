---
title: "13. Presenting research"
layout: default
nav_order: 14
---

# Presenting research
{: .no_toc }

*~8 min read*

**Interview occasional**

## Why it matters

A research interview often *is* a presentation: your project, or a case you were given an hour ago. The failure mode is a cumulative-return chart and a Sharpe, with the mechanism implied. A usable memo leads with the decision, shows the ugly years, and answers the question the listener is already asking — "is this just value, and how many things did you try?"

## Core concepts

- **Lead with the decision.** Reject, paper-trade, or collect more data. Then the mechanism in two sentences, then the evidence. A listener who hears the Sharpe first will spend the rest of the meeting trying to knock it down. Hand them the kill criterion yourself.
- **One table beats five charts.** Sample window and frequency, universe rule, lag from signal to fill, gross and net Sharpe or IR, turnover, max drawdown, a factor-adjusted intercept or a clear "not adjusted," and the trial count or the holdout status. If a number needs a footnote to be true, the footnote is the result.
- **Show the path.** By year, or at least through one stress window. A single equity curve that starts at the friendliest date is a chart crime even when the math underneath is fine. Cumulative wealth with no benchmark and no cost assumption is advertising.
- **Say what would change your mind.** Industry neutralization kills it. Costs of N bps kill it. The holdout is flat. The effect is one sector. Interviewers trust a candidate who names the fragility more than a candidate who defends every month.
- **Anticipate the standard objections.** "Is this momentum or value?" Residualize, or show loadings. "What about 2008 or 2020?" Show those years. "How many specs did you try?" Say the number, or say you do not know and the result is exploratory. Dodging any of these reads as inexperience, not as polish. See [Factor models](../04-factor-models/) and [Multiple testing](../08-statistical-significance-multiple-testing/).
- **Separate exploration from confirmation in the prose.** "We noticed in the first half, and the second half still shows it" is a different sentence from "the full sample Sharpe is 1.3." Use the first sentence if that is what happened.
- **Match the claim to the evidence.** "Tradable alpha" requires a lag, a universe as of t, and a cost. "Interesting gross pattern" is allowed to be weaker, and it is a better claim when that is all you have. Over-claiming a notebook result is the fastest way to lose the room.

## Mental model

```
decision (kill / keep / not yet)
    -> mechanism (why it would pay, and to whom)
    -> design (as-of, lag, costs, what you froze)
    -> evidence (path, net, factors, what failed)
    -> what would change the decision
```

If the first slide is a cumulative curve, the rest of the deck is an apology.

## Interview questions

1. **You have 90 seconds to present a project. What do you say, and what do you leave out?**
   Answer: Mechanism, universe and lag, net result versus gross, the main factor exposure, and the main way it fails. Leave out the library stack, every robustness variant, and a precise Sharpe to two decimals. Offer the table if they want depth.

2. **The cumulative return looks smooth and the interviewer asks "what did you not show me?" Name the likely missing panels.**
   Answer: By-year or drawdown path, net of costs, factor loadings or a hedged residual, turnover, and the count of specifications tried. Also the universe rule: survivors versus point-in-time members. A smooth gross curve of current index members is the usual thing being hidden by accident.

3. **Your holdout is weaker than the in-sample period. How do you present that without either burying it or abandoning a real effect?**
   Answer: Put both numbers in the first table. Say whether the holdout was touched. If the sign matches and the magnitude shrank, say "the effect is smaller out of sample, which is what selection would do" and give the net figure you would actually underwrite. Don't average them into one friendlier Sharpe.

4. **A PM says "everyone already knows about momentum, so this is not research." How do you answer?**
   Answer: A known premium can still be a portfolio you understand, size, and cost correctly. The research claim has to be incremental: a better horizon, a cleaner hedge, a capacity or cost result, or evidence the premium is not what you thought. Quoting a rediscovery of 12-minus-1 momentum as new alpha is not a response. See Asness on why a known strategy can still pay, and don't hide behind it if you have no incremental test.

## Watch

- [AQR's Cliff Asness on Meme Stocks, Market Timing, Private Assets & More](https://www.youtube.com/watch?v=lhCL4R3JFOc) — Bloomberg's Money Stuff podcast. Includes how he talks about explaining factors, publishing, and what you should charge for something that is actually alpha. Useful as a model of plain speech, not as a slide template.

## Further reading

- [How Can a Strategy Still Work If Everyone Knows About It?](https://www.aqr.com/Insights/Perspectives/How-Can-a-Strategy-Still-Work-If-Everyone-Knows-About-It) — the objection you will hear, and a serious version of the answer.
- Harvey, Liu, and Zhu, "… and the Cross-Section of Expected Returns." [NBER w20592](https://www.nber.org/papers/w20592). The other standard objection: your t-stat is one of hundreds.
