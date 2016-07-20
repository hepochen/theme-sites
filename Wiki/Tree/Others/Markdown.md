---
title: Markdown解析引擎
---

## 段落
我们在解析 Markdown 的时候，并没有采用常见的`p+br`的模式，而是`p+span+br`。
每个 P 元素，会有一个 class `md_block`。
每个 P 元素内的每一行，会尽量用 SPAN 进行包裹，并有一个 class `md_line`。如果当前行是以`<`开头，`>`结尾的，则认为内部有其它 HTML 元素，同时还会有一个 class `md_line_dom_embed`。
如果没有特别的 css 样式定义，那么这个跟常见的`p+br`的实际效果一致；但是允许更多自定义的可能，比如需要针对中文的行首缩进，可以让`.md_line{display:block; text-indent:2em}`这样的规则生效。

## 代码高亮
除了一部分比如 table、mathjax、flow 等会被直接处理转义的之外，其它的代码块最终会以代码高亮的形式输出。
默认的 class 为`codehilite highlight`， 如果是显示行数的，同时还有一个 class 是`with_lines`。
如果系统中无法对应一个语言的高亮，那么会输出原始的`pre`结果，但是这个`pre`元素，会有个 class为`lang_<所声明的语言>`。

## 特殊标记的 class
### md_align_center
Dom 类型: span
作用: `[居中语法]`产生的标识
特别说明: 如果是`## [title]` 这种H1-H6级别的标题，则拥有`md_align_center`这个 class 之外，也同时会被标记为`md_header`。

### md_align_right
作用类似于`md_align_center`

### todo_item
Dom 类型: LI
作用: `[x] done` & `[ ] undone` 这个Markdown 语法产生的。
特别说明: 如果是 unone 的状态，还会同时拥有一个 class `todo_undone_item`； 反之已完成的则是 `todo_done_item`。

### md_has_block_below
Dom 类型: P
作用: 表示紧跟其后的将是`ul、ol、blockquote、img`的元素。
特别说明: 如果后面是`ul`，则跟`md_has_block_below`同时出现的有`md_has_block_below_ul`; `ol、blockquote、img`也由此可推。

### toc
Dom 类型: DIV
作用: `[TOC]`语法产生的 HTML 片段，由`<div class="toc"">`进行包裹。

### md_scaled_image
Dom类型: IMG
作用: 如果图片实在 MarkEditor 中缩放过的，则会有这个 class。

### x2_image
Dom 类型: IMG
作用: 如果一个图片的 src，末尾是`@2x`的，则会有这个 class 对应。

### x3_image
类似`x2_image`

### x4_image
类似`x2_image`

### md_video
Dom 类型: VIDEO
作用: 使用链接的语法，如果是 mp4文件，会解析为 video。
特别说明: 会同时拥有`md_compiled`这个 class

### md_audio
Dom 类型: AUDIO
作用: 使用链接的语法，如果是 mp3文件，会解析为 audio。
特别说明: 会同时拥有`md_compiled`这个 class


