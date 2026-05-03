# Brain Hub

Engineering and design intelligence documentation, published with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## Prerequisites

- **Python 3.10+** (3.11 or 3.12 recommended)

## Setup

From the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install mkdocs-material
```

`mkdocs-material` pulls in MkDocs and the Markdown extensions this project uses (PyMdown, etc.).

## Run locally

Start the dev server with live reload (edits under `docs/` refresh in the browser):

```bash
mkdocs serve
```

Open **http://127.0.0.1:8000/** (MkDocs prints the exact URL).

For a different host or port:

```bash
mkdocs serve -a 0.0.0.0:8000
```

## Build static site

Generate the static site into the `site/` directory:

```bash
mkdocs build
```

Serve the built output locally (optional):

```bash
python3 -m http.server --directory site 8080
```

Then open **http://127.0.0.1:8080/**.

## Project layout

| Path | Purpose |
|------|---------|
| `mkdocs.yml` | Site name, theme, navigation, and Markdown extensions |
| `docs/` | Source Markdown, images, and `stylesheets/extra.css` |
| `site/` | Build output (from `mkdocs build`; safe to regenerate) |

## Edit the site

- **Configuration & nav:** `mkdocs.yml`
- **Pages:** Markdown files under `docs/`
- **Branding / social links:** `extra.social` in `mkdocs.yml`

After changing `mkdocs.yml`, restart `mkdocs serve` if the dev server does not pick up the change.
