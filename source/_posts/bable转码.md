---
layout: post
title: "Bable转码器用法"
date: 2016-06-06 22:12
comments: true
tags: 
	- 转码 
	- js
	- ES6
---

[Babel](https://babeljs.io/)是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。这意味着，你可以用ES6的方式编写程序，又不用担心现有环境是否支持。下面是一个例子。

```
// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
  return item + 1;
});
```

头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。

<!-- more -->

### 配置文件.babelrc

当然，这个index.html里涉及了localStorage写操作。

```
var s = "";
//慢慢来，别写太大了，好害怕…
for(var i=0; i< 3 * 1024 * 1024; i++){
  s += "0";
}
localStorage.setItem("testData", s);
```

Babel的配置文件是**.babelrc**，存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。
该文件用来设置转码规则和插件，基本格式如下。



```
{
   "presets": [],
   "plugins": []
 }
```



presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

```
# ES2015转码规则
 $ npm install --save-dev babel-preset-es2015
```

```
# react转码规则
 $ npm install --save-dev babel-preset-react
```

```
# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
 $ npm install --save-dev babel-preset-stage-0
 $ npm install --save-dev babel-preset-stage-1
 $ npm install --save-dev babel-preset-stage-2
 $ npm install --save-dev babel-preset-stage-3
```

然后，将这些规则加入**.babelrc**。

```
 {
  "presets": [
  "es2015",
  "react",
  "stage-2"
],
  "plugins": []
}
```



注意，以下所有Babel工具和模块的使用，都必须先写好**.babelrc**。



参考：[es6 阮一峰](http://es6.ruanyifeng.com/#docs/intro)