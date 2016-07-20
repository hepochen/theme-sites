---
title: "Site"
position: 7.1
---
> Site 对象，在直接调用`site`的时候，指向当前站点，另外也是`account.sites`获得列表的单个数据类型。
## 常用属性
### id
站点 ID
### title
网站标题
### configs
作用: 网站设置中填入的各项配置
数据类型: `dict`
### account_id
站点对应的账户 ID
### utc_offset
相对0时区的偏移，(-12.0~+12.0)
### domain
网站(单)域名
### domains
作用: 当前网站申明的域名
数据类型: `list`

## HTML 构建属性
### socials
网站设置中配置了 Social ID，比如 twitter、微博等信息的，形成的 HTML 片段。
### nav
网站设置中配置了导航栏条目，以及设定了`status="page"`的文章共同形成的导航性质的 HTML 片段。
### footer
网站设置中配置了底部栏的 HTML 片段。

## 其它函数调用
### edit
作用: 可以对网站的基本信息进行编辑
注意: 需要网站所有者出于登录时才能生效
### delete
作用: 删除网站，需要输入账户密码进行确认
注意: 需要网站所有者出于登录时才能生效
### show_token
作用: 显示网站 Token 等相关的基本信息
注意: 需要网站所有者出于登录时才能生效

## configs
> `site.configs` 里的各项，实际上是不固定的，由模板设计者、使用者自行可以调配的；本文档仅对一些常规、默认的属性项进行说明。
> 有些属性实际上的值是`None(不存在)`，因为尚未被用户配置过的原因。
### title
网站标题
### sub_title
二级标题
### keywords
一般无实质意义，主要针对 SEO；前提是在模板中被调用
### description
同`keywords`
### posts_per_page
每页文章数，针对输出文章列表的一些函数调用起作用
### images_per_page
每页图片数，针对输出图片列表的一些函数调用起作用
### post_content_type
`plain` or `markdown`, 默认是`markdown`格式；如果是`plain`，则调用`post.content`这个属性的时候，会忽略 Markdown 语法(插图除外)。
### post_paragraph_indent
`整数`类型，单位`em`，表示文章的首行缩进。
### hide_post_prefix
`True/False`，文章的 URL 一般是`/post/xxxxx`，如果设定值为`True`，则对应的 URL 是`/xxxxx`
### mathjax & flowchart & echarts
默认皆`False`，如果设定为`True`，则会载入对应的前端 JS 脚本，以渲染 Mathjax、Flowchat、Echarts 对应的片段。
### theme
网站应用的主题名，类似`Wiki/Fit`，默认值一般为`Blog/Default`
### cache_strategy
站点缓存策略
### force_ssl
如果为`True`，则 `http://`形式访问的，强制跳转到对应的`https://`上。
### hide_comments
如果为 `True`，隐藏系统默认的评论系统。
### nav
用户设定的导航项，数据类型`list`
### footer
用户设定的页尾 HTML 代码片段
### inject_template
用户设定的模板片段，可以被模板引擎渲染，而`footer`是纯 HTML 片段。
### subdomain
数据类型`list`，用户自行设定的，单个文档与二级域名的匹配规则。





