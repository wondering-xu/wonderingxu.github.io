# Notion Flow 内容结构与字段模板

本仓库已为 Notion → Hexo 的同步流程准备了规范与模板。按照以下约定，在 Notion 中录入内容并经由同步脚本生成 Markdown 后，即可直接被 Hexo 使用并生成与线上一致的页面。

## 1) Notion 数据库字段设计（Blog）

创建一个名为「Blog」的 Notion 数据库，包含以下字段：

- Title（标题，类型：Title）
- Slug（短链，类型：Text，仅小写字母/数字/短横线）
- Date（发布日期，类型：Date）
- Tags（标签，类型：Multi-select，多选）
- Summary（摘要，类型：Text）
- Cover（封面图 URL，类型：URL 或 Files）
- Status（状态，类型：Select：Published/Draft）

## 2) 文件命名与 Front Matter 映射

- 文件命名：{date:YYYY-MM-DD}-{slug}.md
- Front matter（YAML）：

```yaml
---
# 必填
title: "文章标题"
slug: "short-slug"
date: "2025-07-17"      # ISO 日期，或完整时间：2025-07-17 10:12:48

# 选填
# 注意：Hexo 中 tags 建议为数组
#      description 将用于 SEO 的 meta description
#      cover 可用于主题封面图扩展（不影响默认渲染）
tags: [tag1, tag2]
description: "一句话摘要"
cover: "https://example.com/cover.jpg"
---

正文内容从这里开始……
```

- 过滤规则：仅当 Status = Published 时才同步与导出。

> 提示：Draft 状态下的文档不会导出到 Hexo 的 `source/_posts` 目录。

## 3) Hexo 兼容性与站点结构

- Permalink：默认的 Hexo 永久链接即可（/:year/:month/:day/:title/）。
- 目录/标签页：本仓库已提供静态的 `/categories/` 与 `/tags/` 导航入口及页面骨架；当文章包含分类/标签并由 Hexo 生成时，页面会自动生效。
- 本仓库导航已添加：Home / Archives / Categories / Tags。

如需在本地基于 Hexo 源码运行（而非本静态产物），建议在 Hexo 的 `_config.yml` 中确认：

```yaml
permalink: :year/:month/:day/:title/
# 建议启用标签/分类页面（需安装相应插件或主题支持）
```

## 4) 示例 Notion Flow 配置（供同步脚本参考）

可供参考的映射配置（JSON）：

```json
{
  "filter": { "property": "Status", "equals": "Published" },
  "filename": "{date:YYYY-MM-DD}-{slug}.md",
  "frontMatter": {
    "title": "Title",
    "slug": "Slug",
    "date": "Date",
    "tags": "Tags",
    "description": "Summary",
    "cover": "Cover"
  },
  "defaults": { "layout": "post" },
  "output": { "root": "source/_posts" }
}
```

> 说明：`frontMatter` 的键为 Markdown 需要写入的字段，值为 Notion 数据库中的字段名称。

## 5) 验证流程

1. 在 Notion「Blog」数据库中新增一条记录：填写 Title、Slug、Date、Tags、Summary、Cover，并将 Status 设为 Published。
2. 运行你的 Notion → Hexo 同步脚本（或集成平台的 Notion Flow），生成 Markdown 文件到 Hexo 的 `source/_posts` 目录，文件名应形如：`2025-07-17-your-slug.md`。
3. 在本地 Hexo 工程中执行：
   - `hexo clean && hexo g && hexo s`
   - 浏览器访问 `http://localhost:4000/` 进行预览，确认首页、文章页、归档页、标签/分类页正常。
4. 部署到线上后确认构建结果与本地一致。

如果你使用本仓库内的静态构建产物进行展示，Categories/Tags 已提供入口与占位页面，后续由你的 Hexo 构建生成真实索引页即可覆盖。
