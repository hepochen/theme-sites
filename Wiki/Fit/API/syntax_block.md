---
title: 扩展块语法
position: 6
---
## 基本说明
`扩展语法`主要是对某些代码片段进行包裹而产生的。
基本的语法类似:
```jade
+xxx(*args, **kwargs)
    for post in posts
        h2= post.title
```


## 语法扩展
### page
作用: 对内部产生的内容，可以实现滚动自动翻页载入。
接受参数: `<as_ul=False>`，如果 as_ul = True, 则最外部的 DOM 元素是 `ul`，反之则是`div`
代码示例:
```jade
+posts.set_min_per_page(6)
+page(as_ul=True)
	for post in posts: li.post
		span.date= post.date('%Y-%m-%d %H:%M')
		.content.markdown= post.content
```

### font
作用: 对内容产生的内容，自动萃取一个足够呈现字体的 webfont 子集(woff 格式)
接受参数: `<path, name="", title_path="",  title_name"">`
参数说明:
- path: 字体名称(如果是系统内置的)或者自己网站内指定的字体文件名
- name: 一般建议为空，作为新的字体名称，为空的时候，系统会自动生成一个
- title_path: 同 path，但是针对标题的(DOM 元素必须以`title_with_font`为 class)
- title_name: 同 name, 但是针对标题的。

代码示例:
```jade
+font("花园明朝")
    p 茶则作为汉族茶具的一种，在唐代已经有了名分。
```

**注意事项:**
1. 这会产生必要的缓存文件，而占用额外的存储空间
2. 字体萃取会消耗比较多额外的计算资源，这部分会另行计费
3. 可能会由于账户限制， 本接口不可被调用
4. 如果使用自己定义的字体，请确保字体授权没有问题；因为此产生授权的问题可能会对你在本平台上的账户产生影响
5. 由于技术原因，不是所有字体类型都能否支持，一般建议使用`.ttf`格式；如果字体无法萃取子集，并不是我们能够处理的问题。

### browser
作用: 仅仅在指定的浏览器下，内部包含的代码才会起作用
参数: `<family, version=None>`
说明: `family`表示浏览器名称，比如`safari`，一般不区分大小写。如果`family`以`@`开头，则表示严格匹配(大小写敏感)。`version` 一般无需指定，如果指定了，则 version 也要完全对应。
代码示例(仅在 Safari 系列浏览器中生效的样式):
```jade
+browser('safari')
	style(type="text/css")
		.homepage_body .posts_in_home .cjk_punctuation, .post .caption .cjk_punctuation{ margin-left: 13px; }
```

