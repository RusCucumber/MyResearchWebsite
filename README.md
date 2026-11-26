# MyResearchWebsite

![Quarto](https://img.shields.io/badge/Quarto-website-75AADB?logo=quarto&logoColor=white)

Personal research website of Ryuki Matsuura: <https://ruscucumber.github.io/MyResearchWebsite/>

- Built with [Quarto](https://quarto.org/) (originally based on [qtwAcademic](https://github.com/chizapoth/qtwAcademic))
- Published by GitHub Pages from `docs/` on `main`
- Planned migration to the [academicpages](https://github.com/academicpages/academicpages.github.io) theme: #6

## Preview and Render

Requires [Quarto](https://quarto.org/docs/get-started/). No Python or R environment is needed.

```bash
quarto preview   # live preview at localhost
quarto render    # regenerate docs/ (commit it together with the source change)
```

In Zed, the same commands are available as tasks (`Quarto: Preview Site`, `Quarto: Render Site`, `Quarto: Preview Current File`).

> [!NOTE]
> `quarto render` does not work inside the Claude Code sandbox (Sass cache under `~/Library/Caches`). Render in your own terminal.

## Workflow

This repository follows **GitHub Flow**. `main` is protected by the ruleset in `.github/ruleset-main_push_block.json` (pull request required, no force push or deletion), and merging to `main` publishes the site.

1. Create an issue from a template in `.github/ISSUE_TEMPLATE/` (Engineering / General Task).
2. Create a branch named `<prefix>/<descriptive-name>-#<issue-number>` (e.g., `doc/add-sigdial-paper-#12`).
3. Edit the sources and run `quarto render`.
4. Commit with a tag: `[add]`, `[upd]`, or `[fix]` (e.g., `[upd] publications for 2026`).
5. Open a pull request to `main`.

| Prefix   | Description                                          | Issue template   |
|----------|------------------------------------------------------|------------------|
| `feat/`  | New page, layout/theme change, site feature          | Engineering Task |
| `fix/`   | Broken links, rendering or layout bugs               | Engineering Task |
| `doc/`   | Content updates (profile, publications, talks, CV)   | General Task     |
| `chore/` | Environment & tooling updates                        | General Task     |

Issues and pull requests are added to the [Task Manager project board](https://github.com/users/RusCucumber/projects/1) by `.github/workflows/add-to-task-manager.yml` (Category `Private`, Project `NaN`). The workflow needs the `PROJECT_CONTROL_TOKEN` repository secret.

## Coding Agent (Claude Code)

The Claude Code setup is ported from [research-project-template](https://github.com/RusCucumber/research-project-template) (see its `docs/claude_code.md` for details).

- `.claude/settings.json`: plugins, permissions, sandbox, and HTTPS git for Claude (`GIT_CONFIG_*` in `env`; run `gh auth login` once)
- Plugins: oh-my-claudecode, superpowers, genshijin, claude-mem — install them as in the template's `docs/manual_setup_flow.md` (step 5); `claude-mem` needs [Bun](https://bun.sh/)
- Rules (`.claude/rules/`): Quarto sandbox caveats, Obsidian vault usage
- Skills (`.claude/skills/`): `git-strategy` (this repository's workflow), `obsidian-markdown`, `pdf`, `docx`, `doc-coauthoring` (update upstream ones with `gh skill update`)
- Instructions: project-specific ones in the root `CLAUDE.md`; `.claude/CLAUDE.md` is managed by oh-my-claudecode

## Project Structure

```
.
├── index.qmd                  # Home
├── publication.qmd            # Publications
├── fluency_annotation_sys.qmd # Fluency Annotation System
├── projects/                  # Project pages (hidden from navbar)
├── images/                    # Images used by pages
├── data/                      # CV (PDF)
├── styles.css                 # Custom CSS
├── _quarto.yml                # Site configuration
├── docs/                      # Rendered site (generated, served by GitHub Pages)
├── .claude/                   # Claude Code settings, rules, skills
├── .github/                   # Issue templates, labels, ruleset, workflows
├── .zed/                      # Zed settings and tasks
└── CLAUDE.md                  # Project instructions for Claude
```
