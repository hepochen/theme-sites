---
title: Utils
position: 5
---


> utils 对应的是一些常用的扩展函数

### html_comments
作用: 针对某个文档对象(文章、图片)，生成对应的评论列表以及评论区域
接受参数: `<doc>`
示例:
```jade
h2= post.title
+utils.html_comments(doc)
```
### auto_value
作用:  自动转化一些数据类型，比如字符串的整数，会转为整数。

### get_now
返回当前的时间戳

### cjk_s2t
将一段字符串，由中文简体转为中文繁体(但不处理一些习惯用语，比如`软件`->`軟體`)

### cjk_t2s
将一段字符串，由中文繁体转为中文简体