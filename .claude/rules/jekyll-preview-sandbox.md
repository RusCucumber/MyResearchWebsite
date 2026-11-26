---
paths:
  - "_config*.yml"
  - "_pages/**"
  - "_publications/**"
  - "_talks/**"
  - "_includes/**"
  - "_layouts/**"
  - "_sass/**"
  - "_data/**"
---

# Previewing the Jekyll Site from the Claude Code Sandbox

The site is the [academicpages](https://github.com/academicpages/academicpages.github.io) Jekyll theme. GitHub Actions (`.github/workflows/jekyll.yml`) builds and deploys it on every push to `main`; nothing generated is committed.

- The sandbox cannot run the preview: Docker cannot reach its daemon socket, and the system Ruby (2.6) is too old for Jekyll 4. Do not install Ruby or gems to work around it.
- Edit the sources, then ask the user to run `docker compose up` in their own terminal and open <http://localhost:4000/MyResearchWebsite/>.
- `_site/`, `.jekyll-cache/`, `Gemfile.lock`, and `assets/js/main.min.js` are generated and git-ignored. Never commit them or edit them by hand.
- The site is served under `baseurl: /MyResearchWebsite`. In pages and includes, build internal links and image paths with `{% include base_path %}` + `{{ base_path }}/...` (or `| relative_url`), never a bare `/images/...`.
