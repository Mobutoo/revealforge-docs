# revealforge-docs

User documentation site for **ExploCallout** by **RevealForge**, built
with [MkDocs](https://www.mkdocs.org/) and the
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
theme. Published at <https://docs.revealforge.com/> via GitHub Pages.

This repository contains documentation only — no package source, no
`.blend` files, no internal planning material.

## Local development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Build check (used in CI):

```bash
mkdocs build --strict
```

## Deployment

Pushes to `main` trigger `.github/workflows/deploy.yml`, which builds
the site and publishes it to GitHub Pages via the official Pages
Actions.
