---
title: Images
position: 4
---



## 直接调用
`images`作为一个 namespace 存在之外，也可以直接被调用。示例:
```jade    
for image in images
    h2= image.title
    img(src=image.url)
```
此时的 `images` 对应的含义如下:
1. 如果URL 以`/category/`开始，则对应当前路径下的目录的子图片列表
2. 其余情况，均为当前站点的图片列表(默认按时间倒序)


## 获取图片与列表
### get_recent (同get_recent_ images)
作用: 获得最近的图片列表，该函数调用不会产生分页对象。
返回值类型: **list**
接收参数: `<limit=8>`
示例:
```jade
recent_images = images.get_recent(10)
recent_images = images.get_recent(limit=10)
```

### recent_<\d+>
这是对`get_recent`的快捷对应。比如get_recent 的代码示例也可以写成
```jade
recent_images = images.recent_10
```

### get_one (同find_one)
作用: 获得单张图片
接收参数: `path`
返回类型: **Image or None**
示例:
```jade    
my_image = images.get_one("hello.png")
```


### get_categories
作用: 可以获得文件夹的数据对象，一般可作为目录(相册)、分类
接受参数: `<path='', level=None, sort='position'>`
返回类型: **list**
说明:
- path: 位于某个路径下的文件夹，默认是位于
- level: 配合path使用的，表示所查询path的深度，**默认不指定**。如果level=1，表示仅查询到其子目录，level=2，则仅查询子目录的子目录；也接受[2,3]这样形式的值，表示查询所有2级以及3级的目录，但不包含更深的路径，也不包含1级子目录。另外接受`>n `或者`<n` 类型的参数，比如`>1`，表示2级目录或以上。
- sort: 最终返回的数据列表的排序方式
注意: images.get_categories 这个 namespace 下获得的文件夹，必然是包含1张及以上的图片

### get_paginator
作用: 根据指定的 paginator 的 name，获得对应的分页对象。
返回类型: **paginator**

### set_min_per_page
可以限定获取文章列表时，保证每页数最小值；接受一个`整数型`参数。

- - - - -

## 其它变量
### all
所有的图片列表，产生分页对象(名为"all_images")。


### image
作用: 当前页面自动匹配到的**图片对象**
类型: `Image or None`
距离说明:
```jade
// 假设 当前 URL 是 /xxx_any/this/is_image.png
// 并且 /this/is_image.png 有对应的图片，那么下行代码中的 im 将对应到这张图片的数据对象
im = images.image
```

### all_categories
作者: 返回当前站点内所有文件夹的数据对象
数据类型: **list**

### categories
作用: 根据当前页面实际渲染内容，匹配到对应的文件夹数据列表
返回数据类型: `list`
规则对应:
1. 如果`images.category`可以获得数据的，则获得这个 category 下的一级子目录;
2. 获得根目录下1-2级的文件夹，并且每个文件夹需保证其所包含的文章数`>=1`.

### category
作用: 根据当前 URL，匹配到对应的文件夹目录
返回数据类型: `category or None`
示例说明: (当前 url 为 /xxx/hello)
```jade    
cat = images.cateogry
// 如果有一个目录名为`hello`, 则 cat 是一个文件夹数据对象，反之则是 None 值
```


### pager (同 paginator)
作用: 根据当前 images 列表调用，自动对应到的分页对象。
注意: 如果 API 调用，对分页对象的处理跨行比较明显的，可以另行使用`get_paginator`进行获取，以避免不必要的数据冲突。

## 其它函数

### unsplash
作用: 可以从 unsplash.com 上随机获取一张图片
接受参数: `<width=1800, height=1200, save_to='_cache/unsplash', max=10, as_doc=True>`
返回数据类型: `Image or String`

各参数说明:
- width & height: 是指 unsplash.com 上指定的原图大小，一般不需要设置
- save_to: 指定获取到的图片保存到当前站点下的哪个目录
- max: 最多获取多少张图片，当达到最大数量后，就不再下载新图片，而从已存在的图库中随机选取一张
- as_doc: 如果是 True，最终结果返回的是 Image 对象，反之是一个 String 类型的 URL 作为图片的地址。

## Image 对象
> 一个图片列表或者通过 images.get_one 获取的图片对象，都是 Image 类型的数据对象；它有固有的属性和命令可供调用。

### 常用属性

```table
变量名   |   描述
url   |   图片的网址
image_width   |   图片原始宽
image_height   |   图片原始高
comments_count   |   评论数
images_count   |   恒等于1
exif | EXIF 信息
date | EXIF中记录的电子日期优先，若无则是文件最后修改时间
```

### exif
作用: 图片的 EXIF 信息
数据类型: dict
```table
| 变量名 | 描述 |
| make | 设备生产商，如 Apple |
| model | 设备型号， 如 iPhone 4 |
| datetime | 照片拍摄时间， 如 2012:10:17 18:22:14 |
| fn | 光圈值 |
| flash | 曝光模式，整数 |
| focal_length | 焦距 |
| exposure | 曝光（快门）时间 |
| iso | ISO值 |
| program | 拍摄模式，0-8 |
| latitud | 纬度，北纬为正、南纬为负数|
| longitude | 经度，东经为正、西经为负数|
| altitude | 海拔高度 |
| height | 拍摄原图高度|
| width | 拍摄原图宽 |
```

###  resize
作用: 可以对图片进行裁剪处理获得缩略图，并获得对应的图片 URL
接受参数: `<width=None, height=None, fixed=False, quality=86>`
各参数说明:
- width: 最终的最大宽度
- height: 最终的最大高度
- fixed: True/False, 如果同时指定 width & height, 则是最终尺寸固定，而非等比例缩放
- quality: 50~100之间的整数，对 png 类型的最终缩略图无效

注意: 这个会产生新的缓存类型的图片，也会最终同步回当前站点的目录内。

