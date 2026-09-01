+++
title = "Hugo 入门"
description = ""
tags = [
    "hugo",
    "入门",
    "开发",
]
date = "2014-04-02"
categories = [
    "开发",
    "hugo",
]
+++

## 第一步：安装 Hugo

前往 [Hugo 发布页](https://github.com/spf13/hugo/releases)，下载与你的操作系统和架构对应的版本。

把它保存到一个特定位置，下一步我们会用到。

更完整的说明见 [安装 Hugo](/overview/installing/)。

## 第二步：构建文档

Hugo 自带一个示例站点，恰好也是你现在正在阅读的文档站。

按以下步骤操作：

 1. 克隆 [hugo 仓库](https://github.com/spf13/hugo)
 2. 进入仓库目录
 3. 以 server 模式运行 hugo，构建文档
 4. 在浏览器打开 http://localhost:1313

对应的伪命令：

    git clone https://github.com/spf13/hugo
    cd hugo
    /path/to/where/you/installed/hugo server --source=./docs
    > 29 pages created
    > 0 tags index created
    > in 27 ms
    > Web Server is available at http://localhost:1313
    > Press ctrl+c to stop

完成之后，就可以在本地构建上跟随本页的其余部分操作了。

## 第三步：修改文档站点

按 ctrl+c 停止 Hugo 进程。

现在我们重新运行 hugo，这次加上 watch 模式：

    /path/to/hugo/from/step/1/hugo server --source=./docs --watch
    > 29 pages created
    > 0 tags index created
    > in 27 ms
    > Web Server is available at http://localhost:1313
    > Watching for changes in /Users/spf13/Code/hugo/docs/content
    > Press ctrl+c to stop

打开你[喜欢的编辑器](http://vim.spf13.com)，修改其中一个源内容页。比如修改本文件来*修正那个笔误*。

内容文件位于 `docs/content/`。除非另有说明，文件与其 URL 的相对位置一一对应，本例中是 `docs/content/overview/quickstart.md`。

修改并保存这个文件，注意终端里发生了什么：

    > Change detected, rebuilding site

    > 29 pages created
    > 0 tags index created
    > in 26 ms

刷新浏览器，笔误已被修正。

注意这一切有多快。试着在构建完成前刷新页面吧，我谅你也不敢。近乎即时的反馈让你的创造力无需等待漫长的构建。

## 第四步：尽情玩耍

学习一样东西最好的方式，就是把玩它。
