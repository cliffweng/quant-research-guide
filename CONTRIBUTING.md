# Contributing

Thanks for helping improve the guide. A few ground rules:

- **Scope**: one topic per file under `topics/`. Keep each topic readable in ~10 minutes.
- **Template**: follow the structure already used in existing topic files (Why it matters, Core concepts, Mental model, Interview questions, Watch, Further reading).
- **Links must be real**: only link to YouTube videos, papers, and articles you have personally verified exist (open the URL, confirm the title/content). Never guess a video ID or URL. Prefer primary papers, the Ken French data library, AQR research notes, and the Quantopian lecture series, but any reputable, verified source is fine.
- **No invented product direction**: this guide is a public study guide for quant-research workflow and interview prep. It is not a trading system. If you want to propose a new topic or reorganize the curriculum, open an issue first.
- **Non-goals**: do not add live trading, brokerage APIs, quizzes, auth, or a progress backend.
- **Local preview**:
  ```bash
  bundle install
  bundle exec jekyll serve
  ```
  Then open `http://127.0.0.1:4000/quant-research-guide/`.
- **Pull requests**: keep them focused (one topic or fix per PR) and describe what changed and why.
