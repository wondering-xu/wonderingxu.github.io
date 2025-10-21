Notion Flow → Hexo → GitHub Pages（自动发布）

目标
- 支持在 Notion 写作，通过 Notion Flow 同步到 Hexo 源码仓库（本仓库），由 GitHub Actions 自动构建并发布到 wondering-xu/wondering-xu.github.io（main 分支）。

仓库结构与关键配置
- 文章目录：source/_posts/
- 站点配置：_config.yml
  - url: https://wondering-xu.github.io
  - root: /
  - permalink: posts/:year/:month/:day/:title/
  - theme: landscape（CI 中自动安装）
  - index_generator.path: posts（避免与 source/index.html 冲突）
  - archive_dir: post-archives（避免与 source/archives/ 冲突）
  - skip_render：保持对 HTML、CSS 等静态资源的直拷贝（不渲染），Markdown 正常渲染为文章
  - new_post_name: :year-:month-:day-:title.md（与 Notion Flow 文件命名一致）
- CI 工作流：.github/workflows/deploy.yml
  - 触发：对 source/_posts/** 的提交会触发构建
  - 步骤：安装 Node → 安装 Hexo 渲染器与主题 → hexo clean && hexo generate → 推送 public/ 到 wondering-xu.github.io/main
  - 依赖：仓库 Secret GH_TOKEN（或部署秘钥 ACTIONS_DEPLOY_KEY，默认使用 GH_TOKEN）

Notion Flow 配置建议
- 绑定数据库：选择你的文章数据库（含以下字段/属性）
  - title：文章标题（Notion 原生 Title）
  - slug：短链（建议英文/拼音）
  - date：发布日期（Date）
  - tags：标签（Multi-select）
  - summary：摘要（Text）
  - cover：封面（File/URL）
  - Status：状态（Select，Published 表示发布）
- 代码仓库与路径
  - Repository：msmlz7rw（Hexo 源码仓库）
  - Branch：main
  - Base Path：source/_posts
  - Filename Pattern：:year-:month-:day-:slug.md
- 图片
  - 推荐：将图片上传到 Notion 后让 Notion Flow下载至 source/images/{slug}/ 下，并把 Markdown 中的图片链接指向 /images/{slug}/...
  - 或直接使用外链图片（如图床/OSS/CDN）；本仓库也支持静态 HTML/资源的直拷贝。

GitHub Actions 准备
- 在本仓库 Settings → Secrets and variables → Actions 中新增：
  - GH_TOKEN：Personal Access Token（需 repo 权限），允许向 wondering-xu/wondering-xu.github.io 推送
- 可选：也可以使用 ACTIONS_DEPLOY_KEY（将写权限 Deploy Key 添加到 wondering-xu.github.io），并相应调整工作流配置

冒烟测试（已内置 TEST 文章）
1) 通过 Notion Flow 同步一篇文章到 source/_posts（或使用本仓库内置的示例：source/_posts/test-notionflow-smoke-test.md）
2) Push 后，进入 Actions 观察部署任务
   - 会克隆并使用主题 landscape
   - 会安装 hexo 渲染器/生成器
   - 执行 hexo clean && hexo generate
   - 使用 peaceiris/actions-gh-pages 推送 public/ 到 wondering-xu.github.io/main
3) 1–3 分钟后访问：https://wondering-xu.github.io/posts/{year}/{month}/{day}/{slug}/
   - 示例：https://wondering-xu.github.io/posts/2025/10/21/notion-flow-smoke-test/
4) 检查图片是否正常显示（如使用 /images/{slug}/... 或外链图片）

常见排错
- Actions 推送失败
  - GH_TOKEN 未配置或权限不足（需 repo 权限，目标仓库为 wondering-xu/wondering-xu.github.io）
  - 目标仓库/分支配置错误（publish_branch: main）
- 构建报错 Theme not found / Renderer not found
  - CI 中已通过 npm 安装渲染器并 git clone 主题；偶发网络失败可重试
- 文章 404 / 不出现在首页
  - 确认文章 Front-matter 中的 date、title、slug 正确
  - 首页（source/index.html）为静态页面，Hexo 自动生成的文章首页在 /posts/
- 图片 404
  - 建议使用 /images/{slug}/... 绝对路径，或确认 Notion Flow 将图片下载到了 source/images 下

使用说明（以后在 Notion 发布的步骤）
1) 在 Notion 的文章数据库中撰写文章，设置 Status=Published
2) 使用 Notion Flow 同步到 msmlz7rw 仓库的 source/_posts 目录
3) 等待 GitHub Actions 完成构建与发布（约 1–3 分钟）
4) 打开 https://wondering-xu.github.io 查看文章
