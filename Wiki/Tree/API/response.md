---
title: Response
position: 10
---



## 设定 Response 属性
### type (同 content_type & mime_type)
作用: 决定 response 的类型
示例:
```jade
response.type = "application/json"
```

### code (同 status_code & status)
作用: 决定 response 的状态码
示例:
```jade
response.code = 200
```

## 函数调用
### raise_404
作用: 主动触发404(Not Found)页面
接受参数: `<description="">`
示例:
```jade
// 触发一个404页面
+resposne.raise_404()
// 触发404页面的同时，进行错误原因的说明
+resposne.raise_404("just can not find the resource")
```

### redirect
作用: 页面跳转
接受参数: `<url, code=302>`

### response
作用: 强制返回一个 response 以替代当前之前渲染的内容
接收参数: `<content, mime_type='text/html'>`
