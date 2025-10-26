---
title: Notion Flow 占位示例
slug: notion-test
date: 2025-01-01 00:00:00
updated: 2025-01-01 00:00:00
tags:
  - Notion
  - Workflow
description: 使用 Notion Flow 同步的占位文章示例，包含封面与内文图片引用。
cover: /assets/notion/placeholder.png
---

> 这篇文章由 Notion Flow 同步生成，用于验证 Hexo 源仓与发布仓的自动发布流程。

![Notion Flow 占位图片](/assets/notion/placeholder.png)

如果你在 Notion 中发布文章并点击同步，Notion Flow 会在 `source/_posts` 目录产生对应的 Markdown 文件，并将图片下载到 `source/assets/notion` 中。

```shell
# 你可以本地验证构建
npm install
npx hexo clean
npx hexo generate
```
