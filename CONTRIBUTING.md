# Contributing

This site is built with [Zensical](https://github.com/zensical/zensical) from
plain Markdown in `docs/`. Editing a page or adding a new one doesn't require
anything beyond a text editor and (optionally) a local preview.

## Getting set up

1. **Install `uv`** if you don't have it — see the
   [official install instructions](https://docs.astral.sh/uv/getting-started/installation/)
   (a single script, no Python needed first).
2. **Clone the repo:**

   - If you have write access to the DUNE org:
     ```bash
     git clone git@github.com:DUNE/dune-spack-project.git
     cd dune-spack-project
     ```
   - Otherwise, [fork the repo](https://github.com/DUNE/dune-spack-project/fork)
     on GitHub first, then clone your fork and open pull requests from it
     against `DUNE/dune-spack-project`:
     ```bash
     git clone git@github.com:<your-username>/dune-spack-project.git
     cd dune-spack-project
     ```
3. **Install dependencies:**

   ```bash
   uv sync --locked
   ```

   By convention, contributors working across multiple `uv` projects may
   prefer keeping the environment outside the repo instead:
   ```bash
   uv venv ~/venvs/dune-spack-project
   UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project uv sync --locked
   ```
   Either way works — the in-repo `.venv/` is already gitignored.

## Editing an existing page

1. Edit the relevant file under `docs/` (`index.md`, `repositories.md`,
   `configuration-reference.md`, `contributing.md`).
2. Preview locally:

   ```bash
   uv run --locked zensical serve
   ```

   Serves at `http://127.0.0.1:8000` with live reload. (Prefix with
   `UV_PROJECT_ENVIRONMENT=~/venvs/dune-spack-project` if you used the
   outside-repo environment above.)
3. Commit and push your change, then open a pull request against `main`.
   `.github/workflows/docs.yml` builds and deploys the site automatically
   once it merges — no manual publish step.

## Adding a new page

1. Add a new `.md` file under `docs/`.
2. Add it to the `nav` list in `zensical.toml` so it appears in the site
   navigation:

   ```toml
   nav = [
     { "Home" = "index.md" },
     { "Your Page Title" = "your-page.md" },
   ]
   ```
3. Preview, commit, push, and open a pull request as above.

## Style notes

- Keep content plain Markdown; the extensions already enabled (admonitions,
  tables, task lists, footnotes) cover most needs — see `zensical.toml` for
  the full list.
- Prefer linking to the authoritative source (an upstream repo, the official
  Spack docs, etc.) over duplicating content that lives and changes
  elsewhere.
- This site is public-facing DUNE documentation: keep content professional
  and avoid references to internal meetings, tooling, or process details
  that don't mean anything to an outside reader.
