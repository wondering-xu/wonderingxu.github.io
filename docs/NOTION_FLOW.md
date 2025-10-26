# Notion Flow 同步指引

通过 Notion Flow 插件，可以将 Notion 数据库中的文章一键同步到 Hexo 源仓库。本指南帮助你完成一次性的配置，并说明同步后的目录结构与 CI 流程。

## 一次性配置步骤

1. 在 Notion 中安装并打开 **Notion Flow** 插件，登录你的 Notion 账户与 GitHub 账户。
2. 选择存放博客内容的数据库（例如 `Blog` 数据库），授予插件读取权限。
3. 选择目标 GitHub 仓库：`wonderingxu/wonderingxu_github_io`。
4. 设置同步目录：`source/_posts`。
5. 文件命名模板：`{date:YYYY-MM-DD}-{slug}.md`。
6. 勾选 **下载图片到仓库** 并指定目录：`source/assets/notion`。
7. 设置过滤条件：`Status = Published`（只同步发布状态的文章）。

> 初次设置后记得在本仓库的 `Settings > Secrets and variables > Actions` 中添加 `GH_TOKEN`，这是 GitHub Actions 发布静态站点所需的 PAT（需具备 `repo` 权限）。

## 字段映射建议

| Notion 字段 | Markdown Front Matter | 说明 |
|-------------|-----------------------|------|
| `Title`     | `title`                | 文章标题 |
| `Slug`      | `slug`                 | 将用于生成文件名与固定链接 |
| `Date`      | `date`                 | 发布时间，格式 `YYYY-MM-DD HH:mm:ss` |
| `Tags`      | `tags`                 | 标签列表，建议使用多选属性 |
| `Summary`   | `description`          | 用于摘要、社交分享文案 |
| `Cover`     | `cover`                | 插图或封面，将保存到 `source/assets/notion` |

同步完成后，图片资产会自动出现在 `source/assets/notion` 目录，Markdown 文件位于 `source/_posts`。

## 验证与发布流程（冒烟测试）

1. 在 Notion 数据库中新增一条记录：`Title = TEST`、`Slug = test`、`Status = Published`。
2. 点击插件中的 **同步** 按钮，等待仓库出现 `source/_posts/2025-xx-xx-test.md`。
3. 仓库中的 GitHub Actions 会自动运行 `hexo clean && hexo generate`，并将 `public/` 发布到 `wondering-xu.github.io` 仓库。
4. 当 Actions 显示绿色勾号后，1–3 分钟内即可在生产站点看到新文章。

如遇同步失败，可检查：

- 仓库是否设置了 `GH_TOKEN`。
- Notion 记录的 `Status` 是否为 `Published`。
- `Slug` 是否唯一，且文件名未被占用。
- GitHub Actions 日志是否提示权限问题。
