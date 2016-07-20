---
title: URL 匹配
position: 0
---

网站所有的URL都是通过模板的文件路径决定的。
假设某个网站的模板文件路径为`template/post.jade`, 默认对应的是`http://domain.com/post`。


## URL匹配的规则

如果URL是以`.txt` `.mk`  `.md` `.makrodnw`结尾的，则系统会先尝试去寻找对应路径的文本文档，并用`template/markdown.jade`进行渲染；如果这个模板不存在，则继续使用正常的路由逻辑。

1. step1, 访问网站首页时, 匹配index.jade模板.
2. step2, 完整匹配URL, 比如访问`/path/sub_path`匹配`/path/sub_path.jade`.
3. step3, 仅匹配URL的第一部分, 比如访问`/first_path/sub_path`, 但是`/first_path/sub_path.jade`不存在,则会尝试匹配`first_path.jade`.
4. step4, 如果之前步骤都未匹配到，但是存在`site_routes.jade`，尝试匹配之。
5. 以上匹配都失败后,则使用`404.jade`作为`Not Found`的页面。

**注: `URL的第一部分`, 默认是指第一个`/`之前的, 如果URL中存在`~~`, 则取`~~`前的字符作为第一部分.**
```
/path/sub_path 的第一部分是 `path`
/path/sub_path/~~hello/world 的第一部分则是`path/sub_path`
```

## 混合路径的URL

如果模板文件处于`template`目录内，那么文件名中有`+`连接多个关键词，可以实现一个页面匹配多个URL的效果。

比如`template/index+tags+archive.jade`或`template/index+tags+archive.jade`能同时处理`首页`、`/tags`、`/archive`这三个URL。


## 注意事项
1. 文件路径中不要出现`.` `#`以及非英文字符.
- 不要试图使用index.jade完成整个网站的route匹配


## Scss/Sass/Less/CoffeeScript的原生支持
比如`template/styles.scss`，只有访问`http://yoursite.com/template/style.scss?raw=true`的时候，才会保留源格式；其它都是自动编译为CSS进行呈现。
对LESS格式的文件也是同样支持的，但是除了基本的层级关系、变量应用外，对LESS的支持相对有限，比如对应extend的写法并不支持。
另外需要注意的是，编译的过程是忽略所有include/import类似的引用逻辑。
同样，F也支持后缀为`.coffee`的Coffee Script脚本的自动编译。
