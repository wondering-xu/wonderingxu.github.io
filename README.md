> 🚀 **Notion Flow 同步速查卡**
> 1. 在 Notion 中点击 **同步** 按钮。
> 2. 仓库出现 `source/_posts/xxxx-slug.md`。
> 3. 图片保存到 `source/assets/notion/`。
> 4. GitHub Actions 自动运行构建。
> 5. 等待 Actions 任务显示绿色勾号。
> 6. Hexo 会生成最新的 `public/` 内容。
> 7. 自动推送到 `wondering-xu/wondering-xu.github.io`。
> 8. 在发布仓库确认新的部署 commit。
> 9. 1–3 分钟后访问 https://wondering-xu.github.io/ 验证上线。
> 10. 若失败，检查仓库 Secrets 是否存在 `GH_TOKEN`。

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
2. In the repository root, run:
   ```bash
   hexo clean
   hexo generate
   hexo server
   ```
3. Visit <http://localhost:4000/>. The routes `/`, `/about/`, `/guestbook/`, `/photos/`, and `/search/` should match the production site because the HTML is copied as-is.
4. If you do not see updates immediately, use a private/incognito window or perform a hard refresh (Cmd/Ctrl + Shift + R). Pay attention to case sensitivity when linking assets—GitHub Pages is case-sensitive.

## Deployment strategy

Automatic `hexo deploy` is disabled to avoid accidental pushes to production. Recommended workflow:

1. Commit your Markdown/content updates in this repository.
2. Trigger the GitHub Actions workflow manually when you are ready to publish. The workflow will build with `hexo generate` and push the contents of `public/` to the live `wondering-xu/wondering-xu.github.io` repository.
3. Review the published site after the workflow completes.

Avoid pushing directly to the production repository or re-enabling automatic deploy hooks—keeping deployment manual prevents unintentional overwrites of the live site.
