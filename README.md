# porterjenkins.com

Personal website, hosted on GitHub Pages from the `main` branch.

- `index.html` — the home page
- `blog.py` — static blog builder (see below)
- `posts/` — blog sources (Markdown + images), one directory per post
- `blog/` — generated blog pages (committed; GitHub Pages serves them as-is)
- `templates/` — HTML templates for post and listing pages
- `CNAME` — custom domain (`porterjenkins.com`)
- `.nojekyll` — serve files as-is, no Jekyll processing

## Local preview

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Blog

Posts are Markdown files with a front-matter block. Equations use standard
LaTeX (`$...$` inline, `$$...$$` display) and are rendered by KaTeX. Images
on their own line become captioned figures. Code blocks are syntax-highlighted
at build time.

```sh
# Import a markdown file from anywhere; its local images are copied too
uv run blog.py import ~/notes/my-post.md --slug my-post

# Or write directly in posts/<slug>/index.md, then rebuild
uv run blog.py build

# Include posts marked `draft: true`
uv run blog.py build --drafts

# Build and preview in a browser at http://127.0.0.1:8000/blog/
uv run blog.py serve
```

Generated pages use relative paths, so double-clicking `blog/<slug>/index.html`
also renders correctly without a server.

`uv run` installs the dependencies listed at the top of `blog.py` into an
isolated environment. Without uv: `pip install -r requirements.txt` and
`python3 blog.py build`.

Front matter:

```yaml
---
title: A title            # required (falls back to the first H1)
date: 2026-10-12          # optional, defaults to file mtime
summary: One or two sentences for the listing cards.
tags: ml, uncertainty
draft: true               # optional; skipped unless --drafts
---
```

The build renders `posts/` into `blog/<slug>/`, regenerates `blog/index.html`,
and refreshes the cards between the `BLOG:START` / `BLOG:END` markers on the
home page. Commit the generated `blog/` directory along with the source.
