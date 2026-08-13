# Contributing

This site is built with [Zensical](https://github.com/zensical/zensical) from
plain Markdown in `docs/`. Editing a page or adding a new one doesn't require
anything beyond a text editor and (optionally) a local preview.

## Editing an existing page

1. Edit the relevant file under `docs/` (currently just `docs/index.md`).
2. Preview locally:

   ```bash
   uv sync --locked
   uv run --locked zensical serve
   ```

   Serves at `http://127.0.0.1:8000` with live reload.
3. Open a pull request. `.github/workflows/docs.yml` builds and deploys the
   site automatically once your change merges to `main` — no manual publish
   step.

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
3. Preview and open a pull request as above.

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
