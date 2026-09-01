+++
title = "(Hu)go 模板入门"
description = "Hugo 模板语言简介。"
tags = [
    "go",
    "golang",
    "模板",
    "主题",
    "开发",
]
date = 2014-04-02T02:13:50Z
+++

Hugo 的模板引擎使用优秀的 [go][] [html/template][gohtmltemplate] 库。这是一个极其轻量的引擎，只提供非常少量的逻辑。根据我们的经验，这些逻辑刚好足够构建一个优秀的静态网站。如果你用过其他语言或框架的模板系统，会在 go 模板中发现许多相似之处。

本文是 go 模板使用的简明入门，更多细节请参考 [go 文档][gohtmltemplate]。

## Go 模板简介

Go 模板提供了一种极其简单的模板语言。它遵循这样的理念：只有最基本的逻辑才应属于模板或视图层。这种简单带来的一个好处是 go 模板解析非常快。

go 模板的一个独特之处是它是内容感知的：变量和内容会根据其使用位置的上下文进行安全处理。更多细节见 [go 文档][gohtmltemplate]。

## 基本语法

go 模板就是附加了变量和函数的 html 文件。

**go 的变量和函数在 {{ }} 中访问**

访问预定义变量 "foo"：

    {{ foo }}

**参数用空格分隔**

以 1、2 为输入调用 add 函数：

    {{ add 1 2 }}

**方法和字段通过点号访问**

访问 Page 参数 "bar"：

    {{ .Params.bar }}

**可以用括号将项分组**

    {{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}

## 变量

每个 go 模板都有一个可用的结构体（对象）。在 Hugo 中，每个模板都会根据渲染页面的类型传入 page 或 node 结构体。更多细节见[变量](/layout/variables)页面。

通过变量名引用即可访问变量：

    <title>{{ .Title }}</title>

变量也可以先定义再引用：

    {{ $address := "123 Main St."}}
    {{ $address }}

## 函数

go 模板自带少量提供基础功能的函数，同时提供了让应用程序扩展可用函数的机制。[Hugo 模板函数](/layout/functions)提供了一些我们认为对构建网站有用的附加功能。函数通过名称调用，后跟以空格分隔的所需参数。

**示例：**

    {{ add 1 2 }}

## 引入

引入另一个模板时会向其传递它能访问的数据。若要传递当前上下文，请记得加上末尾的点。模板位置总是从 Hugo 的 /layout/ 目录开始。

**示例：**

    {{ template "chrome/header.html" . }}

## 逻辑

go 模板提供最基本的迭代和条件逻辑。

### 迭代

与 go 语言一样，go 模板大量使用 range 来遍历 map、数组或切片：

**示例 1：使用上下文**

    {{ range array }}
        {{ . }}
    {{ end }}

**示例 2：声明值变量名**

    {{ range $element := array }}
        {{ $element }}
    {{ end }}

**示例 3：声明键值变量名**

    {{ range $index, $element := array }}
        {{ $index }}
        {{ $element }}
    {{ end }}

### 条件

`if`、`else`、`else if` 与 `end` 共同构成条件逻辑：

    {{ if isset .Params "alt" }}
        {{ index .Params "alt" }}
    {{ else if isset .Params "caption" }}
        {{ index .Params "caption" }}
    {{ end }}

[go]: https://golang.org/
[gohtmltemplate]: https://golang.org/pkg/html/template/
