# DUNE Spack Project

**Site:** [dune.github.io/dune-spack-project](https://dune.github.io/dune-spack-project/)
(not live yet — see [Status](#status))

Documentation, training material, issue tracking, and project coordination for
Spack and MPD (multi-package development, the `mrb` successor, built as a
Spack extension) development and use across DUNE. Built with
[Zensical](https://github.com/zensical/zensical), rendered as a static site
and published to GitHub Pages via `.github/workflows/docs.yml`.

This is a proof-of-principle skeleton, not the finished site — scoped
narrowly to validate the build/deploy pipeline first. MPD is covered here
rather than in a separate repo since it is itself a Spack extension.

## Local development

By convention this project's environment lives outside the repo:

```bash
# one-time setup
uv venv ~/venvs/dune-spack-project
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv sync --locked

# serve locally with live reload (http://127.0.0.1:8000 by default)
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv run --locked zensical serve

# build the static site into ./site/
UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv run --locked zensical build
```

If you'd rather use a standard in-repo `.venv/`, drop the
`UV_PROJECT_ENVIRONMENT` overrides and run `uv sync --locked` directly.

`zensical serve` builds and serves locally — no GitHub Pages / Actions
configuration is needed for local testing.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for how to edit or add pages.

## Deployment

`.github/workflows/docs.yml` builds and publishes the site to
[dune.github.io/dune-spack-project](https://dune.github.io/dune-spack-project/)
on every push to `main`. It won't run until this repo is pushed to GitHub
with Pages enabled.

## Status

Initial skeleton only, built and reviewed locally. Not yet pushed to GitHub,
so the site link above isn't live yet.

## License

Apache-2.0, matching the convention used by other DUNE Spack repos
(`dune_spack`, `FAQ`).
