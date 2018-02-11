---
layout: post
title: 'javascript获取浏览器基本信息'
---
<!--break-->
## javascript获取浏览器基本信息   


#### 1、Window对象

window.screen.width 、window.screen.height  屏幕的宽度

Window.innerWidth 浏览器窗口的宽度



#### 2、Document对象

document.documentElement ——对应<html>节点

document.body ———对应<body>节点

Element.clientWidth 属性 表示元素的内部宽度，以像素计。该属性包括内边距，但不包括垂直滚动条（如果有的话）、边框和外边距。

```
document.documentElement.clientWidth HTML的宽度 = 浏览器的显示宽度（不包括滚动条，不包括超出显示区域的值）
```



```
document.body.clientWidth 视窗的宽度 等于上面的值
document.body.scrollWidth body的总共宽度
```

注：浏览器刷新的时候  html宽度=浏览器窗口宽度

#### 3、计算浏览器滚动条宽度

```
window.innerWindow - ducument.body.clientWidth  
```

