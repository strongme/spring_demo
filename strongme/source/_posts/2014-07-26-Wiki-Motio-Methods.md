title: Motio-Methods
date: 2014-07-26 13:28:42
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{config.url}}Motio-Home.html)
[Methods English Version](https://github.com/Darsain/motio/wiki/Methods)
---

Motio拥有非常方便且使用的方法。你可以通过一个[**`new Motio`**](https://github.com/Darsain/motio/wiki/Calling)来直接调用它们。

``` javascript
var motio = new Motio(frame, options);
// Play method call
motio.play();
```
<!--more-->
在不设置或者调用特殊方法的情况下，Motio的每次调用都会返回它本身，这样你如果你愿意的话就可以进行链式编程调用了。

``` javascript
motio.on('frame', callback).set('speedX', 50).play();	
```



#### **通过jQuery代理来调用Motio方法**

如果你在使用Motio的jQuery版本插件的话，你还可以像如下演示一样通过jQuery代理对象来进行调用：

``` javascript
$('#frame').motio('methodName' [, arguments... ] );
```

假设已经有一个Motio对象与**`$('#frame')`**进行了关联。这种关联是通过[jQuery插件来调用Motio](https://github.com/Darsain/motio/wiki/Calling#calling-via-jquery-plugin)的时候发生的。

``` javascript
$('#frame').motio('toEnd', callbackFunction);
```

## **Methods**

### **play**

``` javascript
motio.play( [ reverse ] );
```

启动一个只有被另外操作才能打断的持续播放的动画。

- **reverse:** `Boolean` 传入**`true`**值可以使动画按照反方向进行播放。

**`reverse`**树形只在Sprite Based模式下的动画才会起作用。而panning animation的方向是通过传入一个正数或者一个负数设置**`speeX`** 或者是 **`speedY`** 来控制的。

### **暂停 pause**

``` javascript
motio.pause();
```

暂停当前在播放的动画。

### **交替 toggle**

``` javascript
motio.toggle();
```

当前状态为播放便暂停，反之...

### **toStart**

*仅仅在sprite animation模式下起作用*

``` javascript
motio.toStart([immediate][,callback])
```

将动画置于第一帧，并且暂停动画。如果第一帧已经被激活，那么动画将会从最后一帧开始重复播放这个动画。

- **immediate** `Boolean` 是否直接激活动画的最后一帧，并且略过这个动画。
- **callback** `Function` 当动画执行到最后一帧之后要执行的回调函数。

如果在中途这个动画被中断了，那么这个 回调函数就不会被执行。

``` javascript
motio.toStart(true); // Activate first frame immediately without animation.
motio.toStart(callback); // Execute callback when animation reaches the destination.
motio.toStart(true, callback); // Combination of both arguments.
```

### **toEnd**

*仅仅在sprite animation模式下起作用*

``` javascript
motio.toEnd([immediate][,callback])
```

将动画置于最后一帧，并且暂停动画。如果最后一帧动画已经被激活，那么将会从第一帧开始重复播放动画。

- **immediate** `Boolean` 是否直接激活动画的最后一帧，并且略过这个动画。
- **callback** `Function` 当动画执行到最后一帧之后要执行的回调函数。

如果在中途这个动画被中断了，那么这个 回调函数就不会被执行。

``` javascript
motio.toEnd(true); // Activate last frame immediately without animation.
motio.toEnd(callback); // Execute callback when animation reaches the destination.
motio.toEnd(true, callback); // Combination of both arguments.
```

### **to**

*仅仅在sprite animation模式下起作用*

``` javascript
motio.to( frame, [ immediate ] [, callback] );
``` 

让动画执行到指定的帧数，并且暂停动画。

- **frame** `Integer` 要执行到的目标帧数，从`0`开始
- **immediate** `Boolean` 是否里面执行到最后一帧，并且跳过动画。
- **callback** `Function` 当动画执行到最后一帧之后要执行的回调函数。

如果在中途这个动画被中断了，那么这个 回调函数就不会被执行。

如果要跳到的那一帧即是当前被激活的这一帧，那么动画会暂停，并且立马执行回调函数。

``` javascript
motio.to(2, true); // Activate 3rd frame immediately without animation.
motio.to(2, callback); // Execute callback when animation reaches the 3rd frame.
motio.to(2, true, callback); // Combination of both arguments.
```

### **set**

``` javascript
motio.set(name.value);
```

设置指定属性到指定的值。

- **name** `String` 要设置的属性名称
- **value** `Mixed` 要 设置的新的属性值

只有如下的几个树形可以进行动态设置：

- fps
- speedX
- speedY

例如：

``` javascript
motio.set('speedX',10);
```

### **on**

``` javascript
motio.set(eventName.callback);
``` 

为单个或者多个Motio事件设置回调函数。所有可以注册的事件，以及事件接受的参数都可以在这里找到： [Events documentation](https://github.com/Darsain/motio/wiki/Events)。

- **eventName** `Mixed`  事件的名称，或者是回调函数的Map对象
- **callback** `Mixed` 单个回调方法，或者是回调函数数组

例如：

``` javascript
// Basic usage
motio.on('frame', function () {});

// Multiple events, one callback
motio.on('play pause', function () {});

// Multiple callbacks for multiple events
motio.on('play pause', [
	function () {},
	function () {}
]);

// Callback map object
motio.on({
	play: function () {},
	frame: [
		function () {},
		function () {}
	]
});	
``` 

### **off**

``` javascript
motio.off(eventName[.callback]);
```

去掉某个事件的一个、多个或者是全部回调函数。

- **eventName** `Mixed`  事件的名称
- **callback** `Mixed` 单个要移除回调方法，或者是回调函数数组，默认是全部清除

例如：

``` javascript
// Removes one callback from load event
motio.off('load', fn1);

// Removes one callback from multiple events
motio.off('load move', fn1);

// Removes multiple callbacks from multiple event
motio.off('load move', [ fn1, fn2 ]);

// Removes all callbacks from load event
motio.off('load');
```

### **destroy**

``` javascript
motio.destroy();
```

暂停动画，并且将元素的背景图片位置设置到 `0 0`的位置。
