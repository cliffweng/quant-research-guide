# Quant Research Guide

A practical study guide for aspiring quant researchers — research workflow and interview prep for QR / research-intern roles, and for self-study.

**Live site (once GitHub Pages is enabled):** https://cliffweng.github.io/quant-research-guide/

`cliffweng.com` already serves sibling guides, so this guide stays on the GitHub Pages project URL above.

## Roadmap

14 topics, one file each under [`topics/`](topics/), ordered as a research workflow:

1. Research question & hypothesis
2. Data hygiene & survivorship
3. Exploratory analysis
4. Factor models
5. Alpha construction
6. Backtest design
7. Backtest pitfalls (overfit, leakage)
8. Statistical significance & multiple testing
9. Portfolio construction
10. Risk models
11. Execution & TCA intro
12. Research notebooks & reproducibility
13. Presenting research
14. Interview case patterns

## Interview hotspots

Every topic page carries a badge (🎯 Interview frequent / Interview occasional / Background) so you know where to spend prep time. If you're short on time, prioritize these:

- **Data hygiene & survivorship** — "current index members," missing delistings, and restated fundamentals are the standard way a pretty backtest gets thrown out.
- **Factor models** — interviewers ask whether the result is alpha or a known bet (market, value, momentum, sectors). If you can't residualize, you don't have a result yet.
- **Alpha construction** — how a characteristic becomes a score, a horizon, and a spread. Information coefficient, neutralization, and decay show up constantly.
- **Backtest design** — when the signal is known, when you trade, what the portfolio rules are, and which window you refused to touch.
- **Backtest pitfalls** — lookahead, same-bar fills, in-sample tinkering, and costs left at zero. This is the most common "do you believe this Sharpe" question.
- **Statistical significance & multiple testing** — one t-stat after many tries is not evidence. Expect deflated hurdles, false-discovery language, and the Harvey–Liu–Zhu "t greater than 3" point.
- **Interview case patterns** — the loop is usually a case ("here's a signal, walk me through the test"), not a recitation of one formula.

**Occasional** (still worth knowing, less likely to be the whole interview): research question & hypothesis, exploratory analysis, portfolio construction, risk models, notebooks & reproducibility, presenting research.

**Background** (know the shape, rarely probed in depth for a research-intern loop): execution & TCA. You will be asked whether costs can eat the alpha; you will rarely be asked to derive an optimal execution schedule.

This split is a judgment call based on what shows up in QR / research-intern interviews, not a guarantee for any specific loop. A systematic-equity seat will push harder on portfolio construction and risk; an execution seat will push harder on TCA.

## How to use this guide

Each topic page is designed to be read in **~10 minutes** and follows the same structure: why it matters, core concepts, a mental model, 3–5 interview questions with brief answer keys, and a short list of verified YouTube videos. Read them in order, or jump straight to what you need. No backend, no auth, no sign-up — just read the pages.

## How to contribute

See [CONTRIBUTING.md](CONTRIBUTING.md). In short: one topic per file, keep it under ~10 minutes to read, and only link to sources (especially YouTube videos) you've personally verified exist. Open an issue before proposing new topics or restructuring the curriculum.

## Decisions

These are the product locks this guide was built against — echoed here so future contributors don't accidentally relitigate them:

- **Audience**: aspiring QR / research interns, plus self-study. Research workflow and interview prep in one place.
- **Time-boxed**: every topic is readable in 10 minutes or less. Depth is sacrificed for scannability; "further reading" links are where depth lives.
- **Learning + interview prep in one page**: each topic pairs core concepts with interview questions, rather than splitting them into separate tracks.
- **Real links only**: every YouTube link is verified to exist before being added. No invented URLs, ever.
- **Static site, GitHub Pages, Just the Docs**: `remote_theme: just-the-docs/just-the-docs`. `baseurl: "/quant-research-guide"`. `url: "https://cliffweng.github.io"`. No backend, no auth, no quizzes, no progress tracking.
- **Homepage**: https://cliffweng.github.io/quant-research-guide/ — not a custom domain. `cliffweng.com` is already used for sibling guides.
- **Non-goals for v1**: no live trading system, no brokerage APIs, no quizzes, no auth, no progress backend.

## Enabling GitHub Pages

Just the Docs is configured via `_config.yml` (remote theme). After this lands on `main`:

1. Repo **Settings → Pages**.
2. **Build and deployment → Source**: **Deploy from a branch**.
3. Branch: `main` / folder: `/ (root)`.
4. Save. The first build often takes a few minutes.
5. Site publishes to https://cliffweng.github.io/quant-research-guide/

GitHub Pages allows `jekyll-remote-theme`, which is what `remote_theme` in `_config.yml` uses. Do not add a `CNAME` for `cliffweng.com`; that host is already taken by sibling guides.

## Local preview

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://127.0.0.1:4000/quant-research-guide/`.

## License

[MIT](LICENSE)
