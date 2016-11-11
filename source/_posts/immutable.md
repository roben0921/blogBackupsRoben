---
layout: post
title: "immutable.js 技术分享感悟"
date: 2016-09-25 21:20
comments: true
tags: 
	- 数据传递 
	- js
	- 优化


---

周五有一次前端技术分享会，有位同学提出了ImmutableJs。下面我们来了解一下。

#### 什么是Immuutable Data

不可变数据，指一旦被创造后，就不可以被改变的数据。

举个Mutable Data的栗子🌰

```
var data1={a:1,b:2};
var data2=data1;
data2.a=2;
console.log(data1.a)//2
```

什么意思呢？这就是js中的引用类型。js在创建data1变量时，在内存中申请了一块内存，返回变量引用地址。相当于一个老板开了一间店铺拥有一把钥匙。

在创建data2时，并没有在内存中重新申请新的内存，而是和data1共享同一块内存，返回变量引用地址。相当于又来了一个老板又拥有一把钥匙，两个人一起管理这家店铺🏠。

两个变量相互影响，一方修改数据都会影响另一方。相爱相杀。

当然这样也会有一些优点，但是在大型应用开发中，总会埋下一些雷…珍重为好，所以提出了Immutable Data。

<!-- more -->

### JS模拟Immutable Data

原声JS模拟Immutable Data的方法就是深拷贝deep clone。

对了，在分享会上马哥着重强调了一下引用类型的赋值、浅拷贝和深拷贝的区别。

可是后来我发现一个问题啊，网上对浅拷贝的定义发现有歧义，一部分认为是这样的：

```
var obj = { a:1, arr: [1,2] };
var obj1 = obj;            //浅复制
var obj2 = deepCopy(obj);  //深复制
```

另一部分人认为浅拷贝是这样的

```
function extendCopy(obj){
    var newObj={};
    for(var i in obj){
        newObj[i]=obj[i];
    }
    return newObj;
}
```

嗯，我们采取第二种嗯。就酱…

贴段JS模拟ImmutableJS的代码，也就是Deep Clone。

```
function extendDeepCopy(obj,newObj){
    var newObj = newObj || {};
    for(var i in obj){
        if(typeof obj[i] == 'object'){
            newObj[i]=(obj[i].constructor===Array)?[]:{};
            extendDeepCopy(obj[i],newObj[i]);
        }else{
            newObj[i]=obj[i];
        }
    }
    return newObj;
}
```



是的，非常慢非常慢非常慢。每次更改都会克隆所有的节点，天啊，想象一下。

然后，就有了ImmutableJS。

#### Immutable.js出场

Immutable.js的出现源于Functional Programming的思想，即所有数据应该是复制过来，而不是直接修改。

由facebook开源的一个项目，主要是为了解决javascript Immutable Data的问题，通过参考hash maps tries和vector tries提供了一种更有效的方式。

简单的来讲，immutable.js通过structural sharing来解决的性能问题。

![image](http://o77lm3ogm.bkt.clouddn.com/immutable-js-share.gif)

当我们发生一个set操作的时候，immutable.js会只clone它的父级别以上的部分，其他保持不变，这样大家可以共享同样的部分，可以大大提高性能。

举栗子🌰

```
var a={
        a:1,
        b:2,
        c:{
            ca:1,
            cb:{
                cba:2,
                cbb:{
                    cbba:3
                }
            }
        }
    }
```

### Immutable.js分享

周五有一次前端技术分享会，有位同学提出了ImmutableJs。下面我们来了解一下。

#### 什么是Immuutable Data

不可变数据，指一旦被创造后，就不可以被改变的数据。

**举个Mutable Data的栗子🌰**

```
var data1={a:1,b:2};var data2=data1;data2.a=2;console.log(data1.a)//2
```

什么意思呢？这就是js中的引用类型。js在创建data1变量时，在内存中申请了一块内存，返回变量引用地址。相当于一个老板开了一间店铺拥有一把钥匙。

在创建data2时，并没有在内存中重新申请新的内存，而是和data1共享同一块内存，返回变量引用地址。相当于又来了一个老板又拥有一把钥匙，两个人一起管理这家店铺🏠。

两个变量相互影响，一方修改数据都会影响另一方。相爱相杀。

当然这样也会有一些优点，但是在大型应用开发中，总会埋下一些雷…珍重为好，所以提出了Immutable Data。

### JS模拟Immutable Data

原声JS模拟Immutable Data的方法就是深拷贝deep clone。

对了，在分享会上马哥着重强调了一下引用类型的赋值、浅拷贝和深拷贝的区别。

可是后来我发现一个问题啊，网上对浅拷贝的定义发现有歧义，一部分认为是这样的：

```
var obj = { a:1, arr: [1,2] };var obj1 = obj;            //浅复制var obj2 = deepCopy(obj);  //深复制
```

另一部分人认为浅拷贝是这样的：

```
function extendCopy(obj){    var newObj={};    for(var i in obj){        newObj[i]=obj[i];    }    return newObj;}
```

嗯，我们采取第二种嗯。就酱…

贴段JS模拟ImmutableJS的代码，也就是Deep Clone。

```
function extendDeepCopy(obj,newObj){    var newObj = newObj || {};    for(var i in obj){        if(typeof obj[i] == 'object'){            newObj[i]=(obj[i].constructor===Array)?[]:{};            extendDeepCopy(obj[i],newObj[i]);        }else{            newObj[i]=obj[i];        }    }    return newObj;}
```

是的，非常慢非常慢非常慢。每次更改都会克隆所有的节点，天啊，想象一下。

然后，就有了ImmutableJS。

#### Immutable.js出场

Immutable.js的出现源于Functional Programming的思想，即所有数据应该是复制过来，而不是直接修改。

由facebook开源的一个项目，主要是为了解决javascript Immutable Data的问题，通过参考hash maps tries和vector tries提供了一种更有效的方式。

简单的来讲，immutable.js通过structural sharing来解决的性能问题。

![image](http://o77lm3ogm.bkt.clouddn.com/immutable-js-share.gif)

当我们发生一个set操作的时候，immutable.js会只clone它的父级别以上的部分，其他保持不变，这样大家可以共享同样的部分，可以大大提高性能。

举栗子🌰

```
var a={
        a:1,
        b:2,
        c:{
            ca:1,
            cb:{
                cba:2,
                cbb:{
                    cbba:3
                }
            }
        }
    }
```

如果我们修改a.c.cb.cba，对a对象进行特殊深copy，怎么特殊？a.c.cb.cbb不会被copy，还是共享相同内存，为引用类型。因为Immutablejs只会clone它的父级别以上的部分，个人理解，表述错误欢迎指正…

以上就是Immutablejs提高性能的远离，据估计可以提升十倍呢…强大到不能直视…

#### 为什么你要在React.js中使用Immutable Data

熟悉React.js的都应该知道，React.js是一个UI = f(states)的框架，为了解决更新的问题，React.js使用了virtual dom，virtual dom通过diff修改dom，来实现高效的dom更新。

听起来很完美吧，但是有一个问题。当state更新时，如果数据没变，你也会去做virtual dom的diff，这就产生了浪费。这种情况其实很常见。

当然你可能会说，你可以使用PureRenderMixin来解决呀，PureRenderMixin是个好东西，我们可以用它来解决一部分的上述问题，但是如果你留心的话，你可以在文档中看到下面这段提示。

> This only shallowly compares the objects. If these contain complex data structures, it may produce false-negatives for deeper differences. Only mix into components which have simple props and state, or use forceUpdate() when you know deep data structures have changed. Or, consider using immutable objects to facilitate fast comparisons of nested data.

也就是说，PureRenderMixin只是简单的浅比较，不使用于多层比较。那怎么办？？自己去做复杂比较的话，性能又会非常差。

叮咚，方案就是使用immutable.

React 做性能优化时有一个避免重复渲染的大招，就是使用 shouldComponentUpdate()，但它默认返回 true，即始终会执行render()方法，然后做 Virtual DOM 比较，并得出是否需要做真实 DOM 更新，这里往往会带来很多无必要的渲染并成为性能瓶颈。

当然我们也可以在 shouldComponentUpdate() 中使用使用 deepCopy 和 deepCompare 来避免无必要的 render()，但 deepCopy 和 deepCompare 一般都是非常耗性能的。

Immutable 则提供了简洁高效的判断数据是否变化的方法，只需===和is比较就能知道是否需要执行render()，而这个操作几乎 0 成本，所以可以极大提高性能。修改后的shouldComponentUpdate是这样的：



```
shouldComponentUpdate: (nextProps = {}, nextState = {}) => {
  const thisProps = this.props || {}, thisState = this.state || {};
 
  if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
      Object.keys(thisState).length !== Object.keys(nextState).length) {
    return true;
  }
 
  for (const key in nextProps) {
    if (thisProps[key] !== nextProps[key] || ！is(thisProps[key], nextProps[key])) {
      return true;
    }
  }
 
  for (const key in nextState) {
    if (thisState[key] !== nextState[key] || ！is(thisState[key], nextState[key])) {
      return true;
    }
  }
  return false;
}
```

优点：

1. Immutable 降低了 Mutable 带来的复杂度。
2. 节省内存。

Immutable.js使用了 Structure Sharing 会尽量复用内存。没有被引用的对象会被垃圾回收。

```
import { Map} from 'immutable';
let a = Map({
  select: 'users',
  filter: Map({ name: 'Cam' })
})
let b = a.set('select', 'people');
a === b; // false
a.get('filter') === b.get('filter'); // true
```

上面 a 和 b 共享了没有变化的filter节点。

3. Undo/Redo，Copy/Paste，甚至时间旅行这些功能做起来小菜一碟

因为每次数据都是不一样的，只要把这些数据放到一个数组里储存起来，想回退到哪里就拿出对应数据即可，很容易开发出撤销重做这种功能。

4. 并发安全。

传统的并发非常难做，因为要处理各种数据不一致问题，因此『聪明人』发明了各种锁来解决。但使用了 Immutable 之后，数据天生是不可变的，并发锁就不需要了。

然而现在并没什么卵用，因为 JavaScript 还是单线程运行的啊。但未来可能会加入，提前解决未来的问题不也挺好吗？

5. 拥抱函数式编程

Immutable 本身就是函数式编程中的概念，纯函数式编程比面向对象更适用于前端开发。因为只要输入一致，输出必然一致，这样开发的组件更易于调试和组装。

像 ClojureScript，Elm 等函数式编程语言中的数据类型天生都是 Immutable 的，这也是为什么 ClojureScript 基于 React 的框架 — Om 性能比 React 还要好的原因。

