# Jekyll / Docker Commands in the Claude Code Sandbox

- `docker compose up` and `bundle exec jekyll serve|build` do not work inside the sandbox. Edit the sources and ask the user to preview — see `.claude/rules/jekyll-preview-sandbox.md`.
- `sips` (image resizing) also fails in the sandbox (it cannot write its temp file). Ask the user to run it.
- Do not change `_config.yml`, `Dockerfile`, or `docker-compose.yaml` to work around these limits.
