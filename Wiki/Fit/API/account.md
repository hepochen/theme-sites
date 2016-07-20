---
title: Account
position: 7
---


## 基本属性
### id
当前已登录账户的账户 ID
### is_login
`True/False`， 当前是否出于登录状态
### is_admin
`True/False`, 当前登录用户是否是网站管理者(即所有者)
### sites
作用: 当前已登录账户所拥有的站点列表
返回类型: `Site列表`
### clouds
作用: 当前已登录账户所关联的云端服务列表
返回类型: `list`
### delete_cloud
作用: 取消已经关联的某个 Cloud 服务
接收参数: `<cloud_id>`
返回值: 空字符串，相当于不返回
注意: 这个一般要与`account.clouds`配合使用。

## 基本函数
### need_login
作用: 要求当前页面必须出于登录状态才能被访问到
示例: 
```jade
+account.need_login()
```
### need_admin
作用: 跟`need_login`的作用接近，但是必须要出于站长的时候才能被访问到的页面

### login
作用:  登录或者创建一个账户
接受参数: `<callback_url='/'>`
注意: `callback_url`是登录成功之后需要跳转到的页面地址。

### logout
作用: 退出当前已登录的账户

### create_site
作用:  在用户登录的状态下，创建一个网站，并跳转到到某个指定 URL
接受参数: `<redirect_url=None>`
### get_site
作用: 
接受参数: `<site_id>`
