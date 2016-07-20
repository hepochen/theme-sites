---
title: d & data
position: 2
---
> 主要是获取一些(原始)数据的函数。

### get_data
作用: 获得数据的主函数，一般`posts`、`images`获取数据的底层都是由`get_data`获得的。


### get_doc
作用: 获得一个文件的原始数据对象
参数: `<path, match=True>`
返回值: `dict or None`
说明: `match=True`的情况下，路径是必须完全对应；反之则是**前向匹配**，比如`name`可以匹配`name.md`、`name.txt`、`name.jpg` .etc

### has
作用: 判断某个文件、文档是否存在。
参数: `<path>`

### make_tree
作用:  将一个文档列表，根据各自 path (路径信息)，自动归档，如果是 folder 类型的，则会有`children`这个属性。

### config_editor
作用: 主要是配置站点设置用到的函数，可以指定一个配置文件的路径，最终可以形成可以直接操作的 HTML 界面。
参数: `<path, keys, formats=None>`
说明: `path`是具体的配置文件(json 文件)的路径，`keys` 是具体的配置项。
代码示例 (网站的一般配置项):
```jade
{%
    set config_keys= ['title', 'sub_title',  'keywords@long_str',  'description@text',
							'posts_per_page',  'images_per_page',  '-',
							'post_content_type',  'post_paragraph_indent', 'show_toc@bool',                                                                   'hide_post_prefix@bool', '-',
							'mathjax@bool', 'flowchart@bool', 'echarts@bool',]
%}
+d.config_editor("configs/site.json", config_keys)
```

### update_json
作用: 可以指定一个json路径文件，然后更新其部分内容。
参数: `<path, **kwargs>`
示例:
```jade
// 以下代码可以更新`hello.json`这个文件(如果不存在，则自动创建)，更新的内容是 title & content 这两个字段。当然，json 的结构本身是松散的，具体更新什么内容，由具体调用的时候，机动处理。
+d.update_json('hello.json', title="new title", content="new content")
```