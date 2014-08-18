title: Motio-Calling
date: 2014-07-26 00:05:39
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{config.url}}Motio-Home.html)
[Calling English Version](https://github.com/Darsain/motio/wiki/Calling)
---
 
## 创建一个全新的Motio对象

``` javascript
	var motio = new Motio( frame [, options ] );
``` 

这样就实例化了一个可以马上使用的Motio对象。当你新实例化一个Motio对象之后，它的所有特性都是没有启用的，所以调用一个所有属性都是默认值的Motio对象你会得到一张什么也做不了的静态帧。
<!--more-->
新创建的Motio对象也是暂停状态的，所以如果你想立刻启动这个动画，那么你必须在创建好之后去调用**'.play()'**这个方法。


所有可调用的方法列表以及有效的相关文档可以在[Method Documentation](https://github.com/Darsain/motio/wiki/Methods)找到。

---

<!-- more -->

### **frame**

Type: `Element`

有动画背景的Dom 元素。例如：

``` javascript
var motio = new Motio(document.getElementById('frame')); // Native
var motio = new Motio($('#frame')[0]); // With jQuery
```

### **option**

Type: `Object`

配置有Motio特性的Javascript对象。所有属性文档可以在[Options Documentation](https://github.com/Darsain/sly/wiki/Options)找到。

## **通过jQuery插件调用Motio**


``` javascript
$('#frame').motio( [ options ] );
``` 

通过jQuery插件初始化的Motio对象自动激活了动画，所以不需要在创建之后再去手动调用**'.play()'**方法。同时也可以通过**['startPaused'](https://github.com/Darsain/motio/wiki/Options#startpaused)**属性来让它失效，即不再刚创建之后便启动动画。










