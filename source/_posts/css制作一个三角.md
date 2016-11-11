---
layout: post
title: "如何用css写出一个三角"
date: 2016-04-25 21:16
comments: true
tags: 
	- 绘图 
	- css
---

绘制三角形用到的是border，现在讲一下原理。

先来一个div，div的宽度高度均设为0px。只设置border。

css样式

```
.border{
    width：0px;
    height:0px;
    border:solid;
    border-top-width:50px;
    border-top-color:red;
    border-right-width:50px;
    border-right-color:yellow;
    border-bottom-width:50px;
    border-bottom-color:green;
    border-left-width:50px;
    border-left-color:blue;
}
```

<!-- more -->

html元素

```
<div class="border"></div>
```

看一下效果。

<div style="width: 0px; height: 0px; border: solid; border-width: 50px; border-color: red yellow green blue;"></div>

是不是有点懂了。(≖ ‿ ≖)✧

1.上三角。

css样式

```
.triangle-up {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 50px solid green;
    border-top: 0px;
}
```

html元素

```
<div class="triangle-up"></div>
```

也就是说，左，右边界颜色设置为透明就只剩下边界显示的上三角形了吧。看效果。

<div style="width: 0px; height: 0px; border-right: 50px solid transparent; border-bottom: 50px solid green; border-left: 50px solid transparent;"></div>

2.左三角。

css样式

```
.triangle-left {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-bottom: 50px solid transparent;
    border-right: 50px solid yellow;
	border-left: 0px;
}
```

html元素

```
<div class="triangle-left"></div>
```

也就是说，上，下边界颜色设置为透明就只剩下边界显示的左三角形了吧。看效果。

<div style="width: 0px; height: 0px; border-top: 50px solid transparent; border-bottom: 50px solid transparent; border-right: 50px solid yellow; border-left: 0px;"></div>

 剩下的都懂了吧 …一样的道理

3.左上三角

css样式

```
.triangle-topleft {
    width: 0;
    height: 0;
    border-top: 50px solid red;
    border-right: 50px solid transparent;
}
```

html元素

```
<div class="triangle-topleft"></div>
```

设置上边界，在设置右边界为透明色。看效果。

<div style="width: 0; height: 0; border-top: 50px solid red; border-right: 50px solid transparent;"></div>

4.右下边界

css样式

```
.triangle-bottomleft {
    width: 0;
    height: 0;
    border-bottom: 100px solid red;
    border-left: 100px solid transparent;
}
```

html元素

```
<div class="triangle-bottomleft"></div>
```

设置下边界，在设置左边界为透明色。看效果。

<div style="width: 0; height: 0; border-bottom: 50px solid green; border-left: 50px solid transparent;"></div>



很简单吧！！！