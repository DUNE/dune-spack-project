# DUNE Spack Project

Documentation, training material, issue tracking, and project coordination for
Spack development and use across DUNE. Built with [Zensical](https://github.com/zensical/zensical),
rendered as a static site (intended to be published to `dune.github.io/dune-spack-project`
once ready).

Proposed per Heidi Schellman's ask on the DUNE training team Slack, following
July 30, 2026 consensus that a central docs/training/issues home for DUNE
Spack was needed. This is a proof-of-principle skeleton, not the finished
site — scoped narrowly to validate the build/deploy pipeline first.

## Local development

This repo's Python environment lives **outside** the repo, under `~/venvs/dune-spack-project`
(never inside the repo — see home `AGENTS.md`/`CLAUDE.md` convention).

```bash
# one-time setup
uv venv ~/venvs/dune-spack-project
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv sync

# serve locally with live reload (http://127.0.0.1:8000 by default)
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv run zensical serve

# build the static site into ./site/
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv run zensical build
```

`zensical serve` builds and serves locally — no GitHub Pages / Actions
configuration is needed for local testing. A `.github/workflows/` deploy
step (building `site/` and publishing to GitHub Pages) is a later addition,
once this is ready to actually go live under the DUNE org.

## Status

Initial skeleton only, built and reviewed locally. Not yet pushed to GitHub.

## License

Apache-2.0, matching the convention used by other DUNE Spack repos
(`dune_spack`, `FAQ`).
