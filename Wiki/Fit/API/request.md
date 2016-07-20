---
title: Request
position: 9
---


> Request 表示当前用户访问请求相关的信息以及函数调用。

## 常用属性

### request 属性字段
```table
变量名   |   类型   |   描述
host   |   字符串   |   域名
method   |   字符串   |   如`GET` `POST` `PUT`等
form   |   字典   |   通过POST\PUT方式的传值
args   |   字典   |   通过GET方式的传值
values   |   字典   |   合并了form & args
data | 字符串  | POST 方式下原始的内容
json   |   字典   |   客户端发送的mimetype是`application/json`，则此变量存在
xml   |   字典   |   跟json字段功能类似，对应的 mimetype 是`application/xml`
language   |   小写字符串   |   当前访客浏览器的语言，比如`zh_cn`
path   |   字符串   |   -
url   |   字符串   |   -
base_url   |   字符串   |   -
url_root   |   字符串   |   -
url_without_host   |   字符串   |   -
protocol   |   字符串   |   http or https
mime_type | 字符串 | 根据当前 url 推断得到的 mime_type
ext | 字符串 | 根据当前 url 推断得到了文件名后缀(不包含`.`)
is_https   |   布尔值   |   True/False
is_login   |   布尔值   |   当前用户是否已登录
is_admin   |   布尔值   |   当前用户是否是站长
```

### request.path 相关说明
假设`http://www.example.com/page.html?x=y` 被访问，则request有以下几个属性:
```table
变量名   |   值
path   |   /page.html
base_url   |   http://www.example.com/page.html
url   |   http://www.example.com/page.html?x=y
url_root   |   http://www.example.com/
request.url_without_host   |   /page.html?x=y
```

## 其它函数调用
### get_offset_path
作用: 根据当前 URL，得到一定偏移值的路径，。
接收参数: `<offset=1>`
原理:  将当前的 URL 路径去除分页信息后，以`~~`， `/`作为分割，然后按照偏移量重新组装。
注意: `分页信息`是指 URL 如果路径以`/page/<int>`结尾的，则作为当前页码数，先行提取另行处理。
示例: 
```jade
// 假设 URL 为 /hello/world/~~test/hi, 有~~存在，则以~~为分隔符
// 则 path1 将是 test/hi
path1 = request.get_offset_path(1) 

// 假设 URL 为/hello/world/test/hi
// 则 path1 将是 world/test/hi ; path2 将是 test/hi ; path3 将是 hi
path1 = request.get_offset_path(1) 
path2 = request.get_offset_path(2) 
path3 = request.get_offset_path(3) 
```

### get_offset_path 衍生
也可以直接`request.path<int>`的方式调用`get_offset_path`，比如`request.path1`等价于`request.get_offset_path(1)`，以此类推。

### redirect
作用: 页面跳转
接受参数: `<url, code=302>`

###  get_mime_type
作用: 指定一个 URL 路径，推断对应的 mime_type
接受参数: `<path>`

### get_ext
作用: 指定一个 URL 路径，推断对应的文件后缀
接受参数: `<path>`

