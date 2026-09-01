+++
title = "创建一个新主题"
description = "教你如何为 Hugo 创建一个简单的主题。"
date = 2014-09-28T02:13:50Z
+++

## 简介

本教程将展示如何为 Hugo 创建一个简单的主题。我假设你熟悉 HTML、bash 命令行，并且会用 Markdown 排版内容。我会讲解 Hugo 如何使用模板、如何组织模板来创建主题，但不涉及 CSS 样式。

我们先从一个非常基础的模板新建站点开始，然后加入一些页面和文章。在此基础上稍加变化，你就能创建各种类型的网站。

本教程中，你输入的命令以 "$" 提示符开始，输出紧随其后；以 "#" 开头的行是我添加的注释；更新文件时，最后一行的 ":wq" 表示保存退出。

## 创建新站点

    $ hugo new site ~/tmp/zafta
    $ cd ~/tmp/zafta
    $ ls -l
    total 4
    drwxr-xr-x  2 mdhender  staff   68 Sep 29 16:49 archetypes
    -rw-r--r--  1 mdhender  staff     0 Sep 29 16:49 config.toml
    drwxr-xr-x  2 mdhender  staff    68 Sep 29 16:49 content
    drwxr-xr-x  2 mdhender  staff    68 Sep 29 16:49 layouts
    drwxr-xr-x  2 mdhender  staff    68 Sep 29 16:49 static
    $

Hugo 会为新站点创建 `config.toml` 文件和四个空目录：`archetypes`、`content`、`layouts`、`static`。

- `content/` 存放网站内容（Markdown 文件）
- `layouts/` 存放控制内容呈现的模板
- `static/` 存放图片、CSS、JS 等静态文件
- `archetypes/` 存放 `hugo new` 命令的默认 front matter 模板

## 覆盖基础模板

Hugo 对模板目录的查找有明确的规则，本教程会覆盖到部分。我们先创建 `layouts/index.html`（首页模板）和 `layouts/_default/single.html`（单页模板）。

`layouts/index.html`：

    <!DOCTYPE html>
    <html>
    <body>
      <h1>zafta</h1>
    </body>
    </html>

构建站点并运行 server 查看：

    $ hugo server --port 1313 --watch
    1 pages created
    ...

浏览器打开 http://localhost:1313 就能看到新首页。

## 创建文章

    $ hugo new post/first.md
    $ cat content/post/first.md
    +++
    title = "first"
    date = "2014-09-29T17:14:19-05:00"
    draft = false
    ++++

front matter 是文件顶部的元数据（TOML、YAML 或 JSON 格式），包含标题、日期等信息。

## 理解 Hugo 的模板查找规则

这是本教程最重要的部分。Hugo 按以下顺序（先匹配先用）查找模板渲染单页：

1. `/layouts/SECTION/LAYOUT.html`
2. `/layouts/SECTION/single.html`
3. `/layouts/_default/LAYOUT.html`
4. `/layouts/_default/single.html`

首页模板的查找顺序：

1. `/layouts/index.html`
2. `/layouts/_default/list.html` 或 `/layouts/_default/index.html`

## 使用数据渲染内容

让模板真正显示文章内容：

`layouts/_default/single.html`：

    {{ partial "header.html" . }}

      <h1>{{ .Title }}</h1>
      {{ .Content }}

    {{ partial "footer.html" . }}

注意 `{{ .Title }}`、`{{ .Content }}` 是页面变量，`{{ partial }}` 用来引入局部模板，末尾的点把当前上下文传给子模板。

## 区分文章与页面

首页用 `.Site.Pages` 遍历所有内容，用 `.Section` 或 `if` 条件可以只显示特定类型（如 `post`），日期也只显示在文章页——可以给 `post` 区段创建单独的模板 `layouts/post/single.html`，它会覆盖 `_default/single.html`。

## 不要重复自己（DRY）

DRY 是一个优秀的设计目标，Hugo 对此支持得很好。写好模板的艺术一部分在于判断何时新建模板、何时更新现有模板。Hugo 让重构变得快速简单，所以晚一点拆分模板也没关系。
