# AGENTS.md

## Project

Personal blog built on the **Chirpy** Jekyll theme (starter layout). Content is Markdown + Liquid; there is no application code, no Node/npm, and no test suite beyond HTML validation.

- Deployed to GitHub Pages by `.github/workflows/` (`jekyll build` → `htmlproofer` → `deploy-pages`) on push to `main`/`master`.
- Theme comes from the `jekyll-theme-chirpy` gem (pinned `~> 7.6` in `Gemfile`); do **not** copy theme files into this repo.
- `assets/lib` is a git submodule (`chirpy-static-assets`), referenced as `assets/lib` in `_config.yml`/`_data`.

## Layout

- `_config.yml` — site identity, URL, social, comments, analytics, collections, permalink rules. Most user-facing settings live here. It is still mostly Chirpy defaults (`title: Chirpy`, placeholder username/email, empty `url`).
- `_posts/` — blog posts. Filename must be `YYYY-MM-DD-slug.md`; permalink is `/posts/:title/` (see defaults block). Front matter: `title`, `date`, `categories`, `tags`, optional `image`, `pin`, `toc`.
- `_tabs/` — top-level navigation pages (`about.md`, `archives.md`, `categories.md`, `tags.md`). Order via front-matter `order`; permalink `/:title/`.
- `_data/` — `contact.yml` (sidebar social icons) and `share.yml` (share buttons).
- `_plugins/posts-lastmod-hook.rb` — sets `last_modified_at` from git history when a post has more than one commit. Requires full git history (`fetch-depth: 0` in CI).
- `assets/img/` — images (avatar is `photo.jpg`, set in `_config.yml`). `index.html` is the home page and must not be removed.

## Commands

Ruby 3.4 + Bundler are required (a devcontainer is provided in `.devcontainer/`). Ruby is not installed in every environment; check `ruby -v` first.

```shell
bundle install
bash tools/run.sh              # serve at 127.0.0.1:4000 with live reload
bash tools/run.sh -p           # production mode
bash tools/test.sh             # production build to _site + htmlproofer (external links disabled)
```

Validate a build before finishing content changes with `bash tools/test.sh`.

## Conventions

- Write content in Markdown with Chirpy/Liquid extensions (prompts, filepath labels, tabs). Keep existing front-matter keys and the `order`/`icon` scheme in `_tabs/`.
- Post URLs depend on the `permalink: /posts/:title/` default — do not change it without updating existing links.
- `_config.yml` has a "not recommended to modify" section at the bottom (kramdown, collections, defaults, sass, compress_html, archives). Leave it alone unless the task requires it.
- `.editorconfig`, Prettier, stylelint and shellcheck/shfmt settings are in `.vscode/`; match surrounding indentation (2 spaces, LF).
- Images go under `assets/img/` and are referenced from the site root, e.g. `/assets/img/foo.png`.
- Do not edit `_site/`, `.jekyll-cache/`, or `Gemfile.lock` by hand; they are generated/ignored.
