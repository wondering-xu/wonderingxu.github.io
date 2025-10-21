Notion Flow → Hexo → GitHub Pages (CI/CD)

This repository is configured to support automatic deployment of a Hexo site built from Markdown content synchronized by Notion Flow.

How it works
- Notion Flow exports posts as Markdown with YAML front matter into the Hexo source repository under:
  - source/_posts/{date}-{slug}.md
  - Images downloaded to source/assets/notion and links in Markdown rewritten to relative paths under /assets/notion
- A GitHub Actions workflow (.github/workflows/deploy.yml) in the Hexo source repo builds the site and pushes the generated public/ folder to the wondering-xu/wondering-xu.github.io repository (this repository) on branch main.

Hexo configuration
- _config.yml contains the minimal configuration required:
  - url: https://wondering-xu.github.io
  - root: /
  - permalink: posts/:year/:month/:day/:title/
  - skip_render: [] (ensure posts are not skipped)
  - new_post_name: :year-:month-:day-:title.md (aligns with Notion Flow filename pattern)

GitHub Actions secrets
- Configure a repository secret GH_TOKEN in the Hexo source repository. It should be a Personal Access Token with repo permissions that allows pushing to wondering-xu/wondering-xu.github.io.
- Alternatively, configure ACTIONS_DEPLOY_KEY (a deploy key added to wondering-xu.github.io with write access) and update the workflow accordingly.

Verification
1) Commit a new Markdown file from Notion Flow to source/_posts with proper front matter (title, slug, date, tags, summary, cover). Ensure images are placed under source/assets/notion and links point to /assets/notion/...
2) Push to main. The workflow will:
   - Install Node and dependencies
   - Run hexo clean && hexo generate
   - Publish public/ to wondering-xu.github.io/main
3) Within 1–3 minutes the post should be visible at https://wondering-xu.github.io.

Notes
- The workflow triggers on changes in source/_posts, source/pages, source/assets, themes, scripts, or Hexo configs.
- If your project uses pnpm or yarn, update the workflow install step accordingly.
