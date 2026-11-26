# MyResearchWebsite

Personal research website of Ryuki Matsuura, built with [Quarto](https://quarto.org/).

- Sources: `*.qmd` pages, `_quarto.yml` (site config / navbar), `styles.css`, `images/`, `data/` (CV PDF)
- Output: `quarto render` writes the site to `docs/`, which GitHub Pages serves from `main` (`https://ruscucumber.github.io/MyResearchWebsite/`). `docs/` is generated — never edit it by hand
- Rendering does not work inside the Claude Code sandbox: edit sources and ask the user to render (see `.claude/rules/quarto-render-sandbox.md`)
- No Python / R environment: pages contain no executed code chunks
- The site is planned to move to the [academicpages](https://github.com/academicpages/academicpages.github.io) Jekyll theme (#6); avoid investing in Quarto-specific tooling
- Follow the `git-strategy` skill for issues, branches, commits, and PRs
