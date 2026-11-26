---
paths:
  - "**/*.qmd"
  - "_quarto.yml"
---

# Running `quarto render` in the Claude Code Sandbox

`quarto render` / `quarto preview` from the Claude Code sandboxed Bash tool fails at the Sass stage with:

```
ERROR: unable to open database file

Stack trace:
    at async Object.openKv (ext:deno_kv/01_db.ts:9:15)
    at async sassCache (...)
    at async compileWithCache (...)
```

## Root cause

Quarto compiles theme SCSS through a Sass cache backed by a Deno KV database file. On macOS this
cache always lives at `~/Library/Caches/quarto/sass` (`darwinUserCacheDir("quarto")` — hardcoded per
OS, not affected by `XDG_CACHE_HOME`). That path is outside the sandbox's writable allowlist, so the
render aborts.

This is a sandbox-only limitation — the user's own terminal and Zed tasks are unaffected. Do not
"fix" this by modifying `_quarto.yml` theme/sass settings, and do not add `~/Library/Caches` to the
sandbox allowlist unilaterally.

## What to do

- Edit the `.qmd` sources, then ask the user to run `quarto render` (or `quarto preview`) outside the
  sandbox. The rendered site in `docs/` is served by GitHub Pages, so the re-rendered `docs/` must be
  committed in the same PR as the source change.
- Do not hand-edit files under `docs/`; they are generated.
