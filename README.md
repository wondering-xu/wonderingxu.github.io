# Wondering Xu Blog – Hexo Source

This repository hosts the Hexo source code that mirrors the current production site. The online GitHub Pages repository remains untouched – you can preview the exact same HTML locally by building from this source tree.

## Directory layout

```
source/
  _posts/            # Markdown articles rendered through the custom theme
  index.html         # Static landing page copied verbatim
  about/, guestbook/, photos/, search/ (static snapshots)
  css/, js/, assets/ # Styles, scripts, images copied directly
  fancybox/          # Third‑party lightbox assets
themes/custom/
  layout/            # Theme skeleton used only for Markdown posts
  layout/partial/    # Navbar & footer partials
```

Static HTML lives directly under `source/` and is skipped during rendering so it is copied to `public/` unchanged. Only Markdown files inside `source/_posts` go through the Hexo pipeline and are wrapped by the theme.

## Writing new posts

1. Create a Markdown file under `source/_posts/` (e.g. `2025-10-05-my-post.md`). You can use `npx hexo new "My Post"` or create the file manually.
2. Include front matter with at least `title`, `date`, and `slug`. Example:
   ```yaml
   ---
   title: Title Here
   slug: title-here
   date: 2025-10-05 10:00:00
   tags:
     - Notes
   ---
   ```
3. Save images inside `source/assets/notion/` or another folder beneath `source/assets/`, then reference them with absolute paths such as `/assets/notion/image.png`.
4. Keep `source/_posts` strictly for Markdown (`.md`) entries—do not place generated HTML or dated folders in this directory.

## Local preview

1. Make sure Node.js 18+ is installed, then install Hexo CLI if you have not already:
   ```bash
   npm install -g hexo-cli
   ```
   (Alternatively, run the commands below with `npx hexo ...`.)
2. Install the project dependencies:
   ```bash
   npm install
   ```
3. In the repository root, run:
   ```bash
   npx hexo clean
   npx hexo generate
   npx hexo server
   ```
4. Visit <http://localhost:4000/>. The routes `/`, `/about/`, `/guestbook/`, `/photos/`, and `/search/` should match the production site because the HTML is copied as-is.
5. If you do not see updates immediately, use a private/incognito window or perform a hard refresh (Cmd/Ctrl + Shift + R). Pay attention to case sensitivity when linking assets—GitHub Pages is case-sensitive.

## Deployment workflow

Any push to `main` that touches content (for example `source/_posts/**`, `source/pages/**`, `source/assets/**`, or theme files) automatically triggers the **Deploy Hexo site** GitHub Actions workflow. The job performs the following steps:

1. Check out the repository and install Node.js 20.
2. Install Hexo dependencies via `npm ci` (or `npm install` when no lockfile exists).
3. Run `hexo clean` followed by `hexo generate` to create the static site in `public/`.
4. Publish `public/` to the live repository `wondering-xu/wondering-xu.github.io` using `peaceiris/actions-gh-pages`.

Populate the repository secret `GH_TOKEN` (or configure `ACTIONS_DEPLOY_KEY`) with credentials that can push to `wondering-xu/wondering-xu.github.io`. The workflow typically finishes within 1–3 minutes of a push. Local deployment scripts remain available for debugging, but day-to-day publishing happens through CI.

## Notion Flow integration

* Notion Flow should export Markdown with YAML front matter to `source/_posts/{date}-{slug}.md`.
* Provide the fields `title`, `slug`, `date`, `tags`, `summary`, and `cover` in the front matter so Hexo can map them directly.
* The Hexo permalink pattern relies on `slug`, keeping online URLs aligned with Notion entries.
* Download images to `source/assets/notion/` and rewrite their paths in Markdown to `/assets/notion/...` so they are deployed with the site.
* Pages beyond blog posts can live under `source/pages/` if needed; they are rendered like standard Hexo pages and deployed automatically.
