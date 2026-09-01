+++
title = "从 Jekyll 迁移到 Hugo"
description = "将博客从 Jekyll 迁移到 Hugo 的步骤。"
date = 2014-04-02T02:13:50Z
+++

## 把静态内容移到 `static` 目录

Jekyll 有一条规则：任何不以 `_` 开头的目录都会原样复制到 `_site` 输出。而 Hugo 把所有静态内容放在 `static` 目录下，因此你需要把它们都移过去。
在 Jekyll 里长这样的目录结构：

    ▾ <root>/
        ▾ images/
            logo.png

应该变成：

    ▾ <root>/
        ▾ static/
            ▾ images/
                logo.png

另外，任何需要放在站点根目录的文件（如 `CNAME`）也应移到 `static`。

## 创建 Hugo 配置文件

Hugo 可以读取 JSON、YAML 或 TOML 格式的配置，也支持自定义配置参数。详见 [Hugo 配置文档](/overview/configuration/)。

## 把发布目录设为 `_site`

Jekyll 默认发布到 `_site`，而 Hugo 默认发布到 `public`。如果你像我一样[把 `_site` 映射为 `gh-pages` 分支上的 git 子模块](http://blog.blindgaenger.net/generate_github_pages_in_a_submodule.html)，可以二选一：

1. 让子模块将 `gh-pages` 映射到 `public`（推荐）：

        git submodule deinit _site
        git rm _site
        git submodule add -b gh-pages git@github.com:your-username/your-repo.git public

2. 或者修改 Hugo 配置，用 `_site` 替代 `public`：

        {
            ..
            "publishdir": "_site",
            ..
        }

## 把 Jekyll 模板转换为 Hugo 模板

这是迁移工作的主要部分。文档是你的朋友：需要回忆 Jekyll 博客的搭建方式时查 [Jekyll 模板文档](http://jekyllrb.com/docs/templates/)，学习 Hugo 的方式时查 [Hugo 模板文档](/layout/templates/)。

提供一个参考数据点：转换我为 [heyitsalex.net](http://heyitsalex.net/) 写的模板花了我不到几个小时。

## 把 Jekyll 插件转换为 Hugo shortcode

Jekyll 有[插件](https://jekyllrb.com/docs/plugins/)，Hugo 有 [shortcode](/doc/shortcodes/)，移植相当简单。

### 实例

举例来说，我在用 Jekyll 时写了一个自定义 [`image_tag`](https://github.com/alexandre-normand/alexandre-normand/blob/74bb12036a71334fdb7dba84e073382fc06908ec/_plugins/image_tag.rb) 插件来生成带标题的图。了解 shortcode 之后，我发现 Hugo 内置的 figure shortcode 恰好能做同样的事：

    {{%/* figure src="/uploads/2014/03/picard-facepalm.jpg" caption="Jean-Luc Picard facepalming" alt="Jean-Luc Picard facepalming" class="full" */%}}

完整的 shortcode 使用方法见 [Hugo 文档](/doc/shortcodes/)。
