---
title: Posts
position: 3
---



## 直接调用
`posts`作为一个 namespace 存在之外，也可以直接被调用。示例:
```jade    
for post in posts
    h2= post.title
    .content= post.content
```
此时的 `posts` 对应的含义如下:
1. 如果默认 URL 带 GET 方式的参数s 的，则认为是以其关键词搜索的文章列表
2. 如果URL 以`/category/`开始，则对应当前路径下的目录的子文章列表
3. 如果URL以`/tags/`或`/tag/`开始，则以其Tag 对应的文章列表
4. 其余情况，均为当前站点的文章列表(按创建时间倒序)
5. **注意:** 此时返回的 posts，每个 post 的 **status=“public”**


## 获取文章与列表
### get_recent (同get_recent_posts)
作用: 获得最近发布的文章列表，该函数调用不会产生分页对象。
返回值类型: **list**
接收参数: `<limit>`
示例:
```jade
recent_posts = posts.get_recent(10)
recent_posts = posts.get_recent(limit=10)
```

### recent_<\d+>
这是对`get_recent`的快捷对应。比如get_recent 的代码示例也可以写成
```jade
recent_posts = posts.recent_10
```
### get_by_status
作用: 可以根据文章作者定义的文章 status 获得文章列表，并且会产生分页对象，名称为“<status>_posts”。
返回值类型: **list**
接收参数: `<status, limit=None>`
示例:
```jade    
my_posts = posts.get_by_status("page")
// 此时直接调用 posts.paginator, 则是 my_posts 产生的; 如果后面再调用 paginator，可以指定名称
my_paginator = posts.get_paginator("page_posts")
```
说明: limit 这个参数可以不用指定，会默认按照用户自己设定的每页分割数自动对应；也可以自行指定一个**不大于300的整数**。

### get_one (同find_one)
作用: 获得单篇日志
接收参数: `path or url`
返回类型: **post or None**
示例:
```jade    
my_post = posts.get_one(path="hello.md")
my_post2 = posts.get_one(url="hello")
```
注意: 这里的`url` 是指文章作者指定的，而不是系统生成的(默认是以/post/开头)。

### search
作用: 可以进行全文检索，并获得对应的文章列表，并且会产生分页对象，名称为“search_posts”。
接受参数: `<keywords, limit=30>`
返回类型: **list**
示例:
```jade    
my_posts = posts.search("名字")
my_posts2 = posts.search(keywords="Hello", limit=100)
```
注意: limit 可以不传入，默认为30，keywords 不可为空，否则返回一个空 list。

### get_categories
作用: 可以获得文件夹的数据对象，一般可作为目录、分类
接受参数: `<path='', level=None, sort='position'>`
返回类型: **list**
说明:
- path: 位于某个路径下的文件夹，默认是位于
- level: 配合path使用的，表示所查询path的深度，**默认不指定**。如果level=1，表示仅查询到其子目录，level=2，则仅查询子目录的子目录；也接受[2,3]这样形式的值，表示查询所有2级以及3级的目录，但不包含更深的路径，也不包含1级子目录。另外接受`>n `或者`<n` 类型的参数，比如`>1`，表示2级目录或以上。
- sort: 最终返回的数据列表的排序方式
注意: posts.get_categories 这个 namespace 下获得的文件夹，必然是包含1篇及以上的文章
### get_paginator
作用: 根据指定的 paginator 的 name，获得对应的分页对象。
返回类型: **paginator**

### set_min_per_page
可以限定获取文章列表时，保证每页数最小值；接受一个`整数型`参数。

- - - - -

## 其它变量
### all
所有的文章列表，产生分页对象(名为"all_posts")。
跟`posts`这个 namespace 直接被调用的区别为: 始终返回所有的文章，不区分位于目录或者 tag 下。

### post
作用: 当前页面自动匹配到的**文章数据对象**
类型: `dict`
规则如下:
1. 在 template 中通过`post.jade`这个模板进行渲染的时候，URL 对应到了一篇文章
2. URL 本身直接是一个 Markdown 后缀的文件名，并且能对应到具体的某篇文章

### all_categories
作者: 返回当前站点内所有文件夹的数据对象
数据类型: **list**

### categories
作用: 根据当前页面实际渲染内容，匹配到对应的文件夹数据列表
返回数据类型: `list`
规则对应:
1. 如果`posts.category`可以获得数据的，则获得这个 category 下的一级子目录;
2. 获得根目录下1-2级的文件夹，并且每个文件夹需保证其所包含的文章数`>=1`.

### category
作用: 根据当前 URL，匹配到对应的文件夹目录
返回数据类型: `category or None`
示例说明: (当前 url 为 /xxx/hello)
```jade    
cat = posts.cateogry
// 如果有一个目录名为`hello`, 则 cat 是一个文件夹数据对象，反之则是 None 值
```


### pager (同 paginator)
作用: 根据当前 posts 列表调用，自动对应到的分页对象。
注意: 如果 API 调用，对分页对象的处理跨行比较明显的，可以另行使用`get_paginator`进行获取，以避免不必要的数据冲突。

### tags
作用: 根据当前 URL 推断得到的 Tags
类型: `dict`
示例如下: 
```
/tags/hello+world  -> ["hello", "world"]
/xxx/hello+world -> ["hello", "world"]
/abc/hello -> ["hello"]
```
注意: 这个推断是并不严谨的推断，如果在非 tags 相关的页面时，请不要进行调用。

### keywords
作用: 根据当前 URL 推断得到的关键词，以 GET 方式浏览时候 `s` 对应的变量。



