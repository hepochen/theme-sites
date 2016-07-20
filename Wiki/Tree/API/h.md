---
title: h & html
position: 1
---
> h 这个 namespace 主要处理跟 渲染、HTML 相关的操作， h 有一个子 space 为 h.html

## 主要函数
### load
作用: 载入前端 css、js、scss、coffeescript 资源，自动生成对应的 HTML 代码。
注意: 相同的资源，相同的页面，最多只能载入一次，重复调用不会重复载入。

### a
作用: 产生 A 标签元素(超级链接)，并且根据当前页面的 URL，给予`selected`等相应的 class name。
参数: `title, url, dom_id=None, must_equal=False, class="selected active current"`
参数说明:
- title: 链接显示的内容
- url: 链接的地址
- dom_id: 一般不需要特别指定，如果指定了，则是对应的 Dom 的 ID
- must_equal: `True/False`，如果是 True，则当前URL 必须要完全匹配；反之则只要前向匹配则可。
- class: 如果当前页面 URL 和参数中的`url`一致对应，则 A 标签元素增加此 class name

### js_view
作用: 可以产生一个链接元素，点击的时候，可以显示图片、在 iFrame 内浏览页面、播放视频。
参数: `title, link, group_id=None, view_type='', height=None, width=None, thumbnail=None`
参数说明:
- title: 显示的文字内容
- url: 链接地址或者图片地址或者视频地址，如果 url 以`.mp4`结尾，view_type 默认作为`video`处理
- group_id: 在 view_type='image'的时候起作用，表示一个组别，可以实现上一张、下一张
- view_type: `image、iframe、 video`，如果没有特别指定，默认作为`iframe`处理
- thumbnail: 缩略图的 url，如果有指定 thumbnail，但未指定 view_type, 则 view_type 默认作为 `image`处理
- height: 高度， 在 video 类型的时候有效
- width: 宽度，在 video 类型的时候有效

### ajax
作用: 在某个指定的 Dom 上点击后，发送 Ajax 请求，并做进一步的 callback 处理
参数: `dom_id, url, method='get', data='', callback_func=''`
参数说明:
- dom_id: 指定的 Dom ID
- url: Ajax 请求的地址
- method: `get or post`
- data: 需要发送的内容
- callback_func: 请求成功后，回调的函数名

### i18n
作用: 国际化的多语言对应
示例:
```jade
// 获得某个 key 的翻译，如果无法匹配，返回 key 本身
+h.i18n('key')
// 设定一个 key 的翻译，未指定语言，则作为默认值
+h.i18n('key', 'I am Key')
// 设定一个 key 的翻译，并指定了语言
+h.i18n('key', '我是 Key', 'zh_cn')
```

### get_a_color
获得一个颜色(常作为背景色)

### realtime_template
当网站内的`/template/`目录下发生变化时，页面进行自动刷新或者 css 样式内容的自动重新渲染。

// ### todo  paginator (同 pager)
// ### simple_form
// ### render_theme_for_doc
// ### get_rows_in_grid

## h.html
> 这是`h`下的一个子 space，更多对应的是接近纯前端的处理逻辑

### 常用属性
```table
名称 | 作用
mobile_metas | 针对 Mobile 设备自适应的 meta 代码片段
site_description | 针对 SEO 的 meta 代码片段
headers | mobile_metas+site_description
```

### back_to_top
作用:  在页面右下角增加`返回到头部`的元素
参数: `<label='△'>`

### auto_toc
作用: 传入一个文章的 dict 类型的数据对象，在页面右侧自动生成一个 TOC 的代码片段
参数: `<post>`
注意: auto_toc 能实现滚动与当前`toc`的自动对应，但前提是已经设定好对应的 css 样式，以及页面的结构不要复杂

### auto_header
作用: 对`class=auto_header`的元素，如果页面产生滚动的时候，到达一定时候自动隐藏与显示。

### fast
作用: 应用InstantClick技术，加快页面的预载入速度。
注意: 这可能会造成网站额外的请求与页面浏览。

### auto_sidebar
作用: 可以对一个指定的`class=sidebar`的元素，进行 sidebar 式的布局。
参数: `side='left', sidebar_name='sidebar', width=300, default_status="hide"`




