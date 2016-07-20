---
title: API与基本语法
---

模板语法主要是量身订制过的 [Jade](http://jade-lang.com/)， Jade 会进一步转为[Jinja2][jinja]的框架语法再行渲染为 HTML。
可以在模板中混用 Jinja2的部分语法，但模板系统本身默认仅支持 Jade 语法。

**系统中的 API 调用的模板生效的一个前提，是必须位于`/template/`目录下**

## namespace
Cloudll 的模板 API 基本上都按照 namespace 分类，一般不存在直接变量。
比如`request.path`所在的 namespace 就是`request`。
注意: namespace 以英文、小写为准。


## Jade语法
如果用传统的角度看待，那么，一个是`后端`，一个是`前端`。所以，我们一直在寻找可以融合前后端的API语法，经过改造的`Jade`语法，基本上满足了我们的设计需求。
[Jade](http://jade-lang.com/)是Node.js世界中的一种Python式缩进的模板语法，经过改造后，FarBox可以将`.jade`文件转化为`Jinja2`的语法。
为了更好地进行编写，你可能需要为自己的代码编辑器/IDE增加`Jade`语法的支持插件。

### HTML标签、id、class
```jade
.class_name this is content
// 默认会创建div标签，最终结果为<div class="class_name">this is content</div>

tag_name.class_name
// 最终结果为<tag_name class="class_name"></tag_name>

p.class1.class2.class3
// 多个class，最终结果为<p class="class1 class2 class3"></p>

a.a_class(href="#", title="this is a link")
// 多个属性，用`,`隔开，最终结果为<a class="a_class" href="#" title="this is a link"></a>

span#dom_id.class_name_1.class_name_2 this is content
// 最终结果为<span id="dom_id", class="class_name_1 class_name_2">this is content</span>
```

### 变量与HTML标签
如果直接将具体的变量与HTML标签关联起来，可参照如下语法。
```jade
node post.content  
//等同于<node>post.content</node>

var = post.content
// 等同于 {%set var = post.content %}, 是一个变量的赋值

node= post.content 
// =号左前无空格，则是将变量转为HTML标签，等同于<node>{{ post.content }}</node>

node= 'post.content'
// 等同于<node>post.content</node>
```

如果HTML标签中有其它属性定义的。
```jade
a(href=post.url)= post.title
// 等同于 <a href={{post.url}}>{{post.title}}</a>

a(href='/post/{{post.url_path}}')= post.title
// 这混合了Jinja2语法，等同于 <a href='/post/{{post.url_path}}'>{{post.title}}</a>
```


### 运行函数
```jade
+response.raise_404()
// 前面增加+号，则不会被转译为HTML的标签，最终的结果为{{ raise_404() }}
```

### 属性调用
```jade
// 如果一个对象以`_`开头，那么我们认为是一次属性的调用
_response.status
// 一般情况下，也可以直接使用`+`作为开头，多数情况下系统会自行判断是运行函数还是返回属性值
+response.status
```

### 使用`:`进行分割
> 有些时候，为了避免没有必要的行数，我们可以将多行合并，特别是`for/if`、多个 dom 嵌套等情况下。
```jade
body
    for post in posts
        if post.title
            h1= post.title
```
**等价于:**
```
body : for post in posts
    if post.title :  h1= post.title
```

### for/if
```jade
head
	if site.title == 'test'
		title= site.title
	elif site.title == 'a test too'
		title= site.title + '!!!'
	else
		title= "blank"
	
body
	for post in posts
		h1= post.title
```

**注意！！**
不要将if/else/for跟其它逻辑混合, 以下语法都是错误的，会导致模板编译无法成功进行。
```jade
if post
    post.content
else no content 
// 这样写的else是无效的，既不是HTML标签，也不是逻辑判断； 要空一行先。
```


## Jade的继承与引用

### extends与include

这分别对应Jinja2语法中的`extends`与`include`。

```jade
extends base
extends base.jade

include include/comments
include include/comments.jade
include include.comments.jade
```

### macro/mixin

`macro` 是Jinja2的概念，`mixin`则是Jade的概念; 在本系统中两者基本等价。

`macro`相当于自定义一个函数，然后在模板中进行调用。

Jinja2是Python语言创建的模板框架，所以`macro`的定义方式，基本接近Python语言中函数的定义。

```jade
mixin create_sidebar(name)
	h1= name
	p this is sidebar
+create_sidebar()
```

如果macro的定义与调用，都在同一个模板页，则后面可以直接使用这个定义出来的函数。

如果处于不同模板页面，则要先行import。

比如
```jade
from minxins import get_url
```


## Jade与Jinja2语法融合

### 常见例子
在.jade的模板文件中，可以在单独一行中使用Jinja2的语法。
```jade
extends base
{% from "mixins" import make_posts_list %}
{{ site.title }}

.entry
	| {{ post.title }} -
	| {{ post.date }}

```


[jade]: http://jade-lang.com
[jinja]: http://jinja.pocoo.org/docs/
[jinja-cn]: http://docs.torriacg.org/docs/jinja2/templates.html
