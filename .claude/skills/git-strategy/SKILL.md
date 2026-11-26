---
name: git-strategy
description: "Git workflow guidance for this website repository. Use when updating pages or publications, changing the site layout, fixing bugs, creating branches, writing commit messages, creating GitHub issues, or opening pull requests. Covers GitHub Flow, branch prefix selection, commit message format ([add]/[upd]/[fix] tags), GitHub issue creation with .github/ISSUE_TEMPLATE templates, and end-to-end PR workflow from branch creation through merge."
---

# Git Strategy Skill

A skill for following the git conventions of this repository (an academicpages Jekyll site, built and deployed to GitHub Pages by GitHub Actions) when implementing any change — from creating a branch to opening a pull request.

## When to Use This Skill

- Updating page content (profile, publications, talks, project pages)
- Changing the site layout, theme, or navigation
- Fixing broken links or rendering bugs
- Tooling or environment changes
- Writing commit messages and opening pull requests

## End-to-End Workflow

1. Identify the correct branch **prefix** from the table below
2. Read the matching issue template file (see mapping below) and use its `body` field labels to construct the issue body
3. Create the GitHub issue with `gh issue create`, filling in each field from the template
4. Create branch: `git checkout -b <prefix>/descriptive-name-#<issue-number>`
5. Edit the sources (`_pages/`, `_publications/`, `_talks/`, `_config.yml`, `_data/navigation.yml`, `images/`, `files/`)
6. Ask the user to preview the site outside the sandbox (`docker compose up`, <http://localhost:4000/MyResearchWebsite/>) — see `.claude/rules/jekyll-preview-sandbox.md`. Nothing generated is committed: GitHub Actions builds and deploys the site after merge
7. Commit: `git commit -m "[tag] short description"`
8. (Optional) For a non-trivial change, get a second opinion before opening the PR: `/oh-my-claudecode:ask codex`
9. Push and open a PR to `main` (HTTPS only — see [Pushing (HTTPS)](#pushing-https)). Merging to `main` publishes the site

## Pushing (HTTPS)

SSH does not work inside the Claude Code sandbox (no ssh-agent socket access, and `~/.ssh/**` is unreadable). `.claude/settings.json` therefore sets `GIT_CONFIG_*` env vars for Claude's commands only: GitHub SSH URLs (`git@github.com:` / `ssh://git@github.com/`) are rewritten to HTTPS (`url.<base>.insteadOf`), and `gh auth git-credential` supplies the token. The user keeps SSH in their own terminal — **do not** change the remote URL or global git/gh config.

- Push with `git push origin <branch>` (**no `-u`**), whether `origin` is SSH or HTTPS. The sandbox cannot write `.git/config`, so `-u` makes the push succeed but then fails with `could not lock config file ... Operation not permitted` — that error does not mean the push failed. If an upstream is needed, ask the user to run `! git branch --set-upstream-to=origin/<branch>`.
- This repository has two remotes (`origin`, `upstream`), so pass `-R RusCucumber/MyResearchWebsite` to `gh` commands that ask for a repository.
- If authentication fails, check `env | grep '^GIT_CONFIG_'` (the `insteadOf` and `gh auth git-credential` entries should be there; Claude Code may append its own entries such as `safe.directory`, so the count can exceed 4. If missing, ask the user to restart Claude Code) and `gh auth status`; if not logged in, ask the user to run `gh auth login` in their own terminal.

## Branching Strategy

This repository uses **GitHub Flow** — all work branches off `main` and merges back via PR.

| Prefix   | Use case                                                 | Linked issue template |
| -------- | -------------------------------------------------------- | --------------------- |
| `feat/`  | New page, layout/theme change, new site feature          | Engineering Task      |
| `fix/`   | Broken links, rendering or layout bugs                   | Engineering Task      |
| `doc/`   | Content updates (profile, publications, talks, CV, README) | General Task        |
| `chore/` | Tooling/dependency/environment tasks                     | General Task          |

**Branch naming rule:** `<prefix>/descriptive-name-#<issue-number>`

- Lowercase, hyphen-separated words
- Always append the linked GitHub issue number at the end
- Examples: `doc/add-sigdial-paper-#12`, `feat/talks-page-#8`, `fix/broken-doi-link-#3`

## GitHub Issue Templates

Two issue templates live in `.github/ISSUE_TEMPLATE/`. **Always create an issue before branching**, using the template that matches the planned branch prefix.

| Template         | File                   | Title prefix | Label                                  | Branch prefixes   | Workflow                                    |
| ---------------- | ---------------------- | ------------ | -------------------------------------- | ----------------- | ------------------------------------------- |
| Engineering Task | `engineering_task.yml` | `[Dev]`      | `Feature` / `Bug` (pick one)           | `feat/`, `fix/`   | branch → implement → preview locally → PR   |
| General Task     | `general_task.yml`     | `[Task]`     | `Documentation` / `Chore` (pick one)   | `doc/`, `chore/`  | branch → implement → PR                     |

### Issue Template Usage (REQUIRED)

Before creating any issue:

1. Read the appropriate template file from `.github/ISSUE_TEMPLATE/` using the Read tool
2. Parse each `label` field under `body` in the YAML
3. Build the `gh issue create --body` content using those labels as section headers, filling in values based on context

**Example — General Task** (`doc/` branch):

```bash
gh issue create -R RusCucumber/MyResearchWebsite \
  --title "[Task] add SIGDIAL 2025 paper to publications" \
  --label "Documentation" \
  --body "$(cat <<'EOF'
## Task Description
Add the EmoNews paper to the publication list and the home page.

## Branch Type
- [x] `doc/`: Content updates (pages, publications, CV)

## Task Details
Add `_publications/2025-12-01-emonews-....md` (category `conferences`) and list it under Selected Publication in `_pages/about.md`.

## Action Steps
- [ ] Create a branch named `doc/add-sigdial-paper-#<issue-number>`
- [ ] Implement the task as described
- [ ] Push the branch, open a PR, and review the changes

## Additional Notes
EOF
)"
```

Always derive section headers from the template's `label` values — do not hardcode them. Pick the single label that best matches the task.

## Commit Message Format

Use short prefix tags before the description:

| Tag     | When to use                              | Example                               |
| ------- | ---------------------------------------- | ------------------------------------- |
| `[add]` | Adding something new                     | `[add] talks page`                    |
| `[upd]` | Updating or enhancing existing content   | `[upd] publications for 2026`         |
| `[fix]` | Fixing a bug                             | `[fix] broken doi link in index`      |

Format: `[tag] short description` — keep the description concise and in lowercase.

## Constraints

- Do not push directly to `main`; always use a PR (`main` is protected by a ruleset and is what GitHub Pages serves)
- Do not use `--no-verify` or skip CI checks; fix root causes
- Do not push over SSH, change the user's git/gh config, or try to work around the sandbox; rely on the Claude-only HTTPS setup (see [Pushing (HTTPS)](#pushing-https))
- Do not commit generated files (`_site/`, `.jekyll-cache/`, `Gemfile.lock`, `assets/js/main.min.js`)
