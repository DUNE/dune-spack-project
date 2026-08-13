# DUNE Spack Project

**Site:** [dune.github.io/dune-spack-project](https://dune.github.io/dune-spack-project/)

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
on every push to `main`.

## Issues

Tracked on the [DUNE Spack & MPD Issues](https://github.com/orgs/DUNE/projects/37)
board (org-wide GitHub Project), aggregating issues from `dune-spack-config`,
`dune-spack-envs`, `dune_spack`, `dune-release-configs`, and
`FNALssi/spack-mpd`. Views: **DUNE+FNALssi** (table, everything), **DUNE
Repos** (table, DUNE-org only), **DUNE Kanban** (board, grouped by status).

To report a problem: open the issue in whichever of those five repos it
belongs to, or in this repo if the right one isn't obvious (Issues are
enabled here too). Then confirm it landed on the board — if it doesn't show
up within a minute or two, add it yourself via the issue's **Projects**
sidebar panel, or `gh issue edit <issue-url> --add-project "DUNE Spack & MPD Issues"`.
You can also add a draft item directly on the board with no issue behind it,
but that's better for quick capture than for anything you want tracked
properly (comments, labels, PR links) — prefer a real issue when you can.
See the site's [Issues section](https://dune.github.io/dune-spack-project/#issues)
for the full walkthrough.

## Status

Initial skeleton, live and building on every push to `main`. Content is a
starting set, not exhaustive.

## Copyright and Licensing
Copyright © 2026 FERMI NATIONAL ACCELERATOR LABORATORY for the benefit of the DUNE Collaboration.

This repository, and all software contained within, except where noted within the individual source files, is licensed under
the Apache License, Version 2.0 (the "License"); you may not use this
file except in compliance with the License. You may obtain a copy of
the License at

    http://www.apache.org/licenses/LICENSE-2.0

Copyright is granted to FERMI NATIONAL ACCELERATOR LABORATORY on behalf
of the Deep Underground Neutrino Experiment (DUNE). Unless required by
applicable law or agreed to in writing, software distributed under the
License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for
the specific language governing permissions and limitations under the
License.
