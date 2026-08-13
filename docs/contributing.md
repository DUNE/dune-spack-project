---
title: Contributing
---

# Contributing

This site is built with [Zensical](https://github.com/zensical/zensical)
from plain Markdown in `docs/`. Editing a page or adding a new one doesn't
require anything beyond a text editor and (optionally) a local preview.

## Getting set up

Install [`uv`](https://docs.astral.sh/uv/) (no Python needed first):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Clone the repo — if you have write access to the DUNE org:

```bash
git clone git@github.com:DUNE/dune-spack-project.git
cd dune-spack-project
```

Otherwise, [fork the repo](https://github.com/DUNE/dune-spack-project/fork)
first, then clone your fork instead and open pull requests from it against
`DUNE/dune-spack-project`.

Install dependencies:

```bash
uv sync --locked
```

## Editing an existing page

Edit the relevant file under `docs/` — `index.md`, `repositories.md`,
`configuration-reference.md`, or `contributing.md` (this page). Preview
locally with live reload:

```bash
uv run --locked zensical serve
```

Serves at `http://127.0.0.1:8000`. Commit, push, and open a pull request
against `main`. `.github/workflows/docs.yml` builds and deploys the site
automatically once it merges — no manual publish step.

## Adding a new page

Add a new `.md` file under `docs/`, then add it to the `nav` list in
`zensical.toml` so it appears in the site navigation:

```toml
nav = [
  { "Home" = "index.md" },
  { "Your Page Title" = "your-page.md" },
]
```

Preview, commit, push, and open a pull request as above.

## Reporting a problem instead

If you've spotted an issue but don't want to fix it yourself, see
[Issues](index.md#issues) on the home page for where to report it.
