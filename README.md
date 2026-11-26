# MyResearchWebsite

![Jekyll](https://img.shields.io/badge/Jekyll-academicpages-CC0000?logo=jekyll&logoColor=white)

Personal research website of Ryuki Matsuura: <https://ruscucumber.github.io/MyResearchWebsite/>

- Built with the [academicpages](https://github.com/academicpages/academicpages.github.io) Jekyll template (MIT, see `LICENSE`), a fork of [Minimal Mistakes](https://mademistakes.com/work/minimal-mistakes-jekyll-theme/). Migrated from Quarto in #6
- Built and deployed to GitHub Pages by `.github/workflows/jekyll.yml` on every push to `main` (Settings → Pages → Source: **GitHub Actions**)
- Served under `baseurl: /MyResearchWebsite`

## Preview

Requires [Docker](https://www.docker.com/). No local Ruby is needed.

```bash
docker compose up --build   # then open http://localhost:4000/MyResearchWebsite/
docker compose down
```

The first run builds the image (Ruby 3.2 + gems). The container builds `assets/js/main.min.js` and runs `jekyll serve` with live reload; changes to `_config.yml` need a restart. In Zed, the same commands are available as tasks (`Jekyll: Preview Site`, `Jekyll: Stop Preview`).

> [!NOTE]
> Docker and Jekyll do not run inside the Claude Code sandbox. Preview in your own terminal.

## Editing Content

| What                  | Where                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------|
| Home (About)          | `_pages/about.md`                                                                              |
| Sidebar profile       | `author:` in `_config.yml`, photo `images/profile.jpg`                                         |
| Publications          | one file per paper in `_publications/` (`category`: `manuscripts` / `conferences` / `nonrefereed`) |
| Talks                 | one file per talk in `_talks/` (`category`: `invited` / `refereed` / `nonrefereed`)            |
| Fluency Annotation    | `_pages/fluency-annotation-system.md`                                                          |
| CV                    | `files/cv_ryuki_matsuura.pdf`                                                                  |
| Navigation bar        | `_data/navigation.yml`                                                                         |

Category headings and their order are set by `publication_category` and `talk_category` in `_config.yml`. Inside pages, write internal links and images as `{{ base_path }}/images/...` after `{% include base_path %}`, so they work under `/MyResearchWebsite`.

## Workflow

This repository follows **GitHub Flow**. `main` is protected by the ruleset in `.github/ruleset-main_push_block.json` (pull request required, no force push or deletion), and merging to `main` publishes the site.

1. Create an issue from a template in `.github/ISSUE_TEMPLATE/` (Engineering / General Task).
2. Create a branch named `<prefix>/<descriptive-name>-#<issue-number>` (e.g., `doc/add-sigdial-paper-#12`).
3. Edit the sources and preview with `docker compose up`.
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
- Rules (`.claude/rules/`): Jekyll/Docker sandbox caveats, Obsidian vault usage
- Skills (`.claude/skills/`): `git-strategy` (this repository's workflow), `obsidian-markdown`, `pdf`, `docx`, `doc-coauthoring` (update upstream ones with `gh skill update`)
- Instructions: project-specific ones in `CLAUDE.local.md` (git-ignored, local only); `.claude/CLAUDE.md` is managed by oh-my-claudecode

## Project Structure

```
.
├── _pages/                    # About (home), Publications, Talks, Fluency Annotation System, archives, 404
├── _publications/             # One Markdown file per publication
├── _talks/                    # One Markdown file per talk
├── _data/                     # navigation.yml, ui-text.yml
├── _includes/ _layouts/ _sass/ assets/   # academicpages theme
├── images/                    # Profile photo, favicons, page images
├── files/                     # CV (PDF)
├── _config.yml                # Site configuration (author, baseurl, categories)
├── _config_docker.yml         # Overrides for the local Docker preview
├── Dockerfile, docker-compose.yaml, Gemfile, package.json
├── .claude/                   # Claude Code settings, rules, skills
├── .github/                   # Issue templates, labels, ruleset, workflows
├── .zed/                      # Zed settings and tasks
└── CLAUDE.local.md            # Local project instructions for Claude (git-ignored)
```
