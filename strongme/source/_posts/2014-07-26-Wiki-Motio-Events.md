title: Motio-Events
date: 2014-07-26 10:56:41
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{config.url}}Motio-Home.html)
[Events English Version](https://github.com/Darsain/motio/wiki/Events)
---
 
你可以用**`.on()`**和**`off()`**方法为Motio注册回调函数：

``` javascript
var motio = new Motio(frame, options);
// Register a callback to multiple events
motio.on('play pause', fn);
// Start playing the animation
motio.play();
```
<!--more-->
可以在 [on & off methods documentation](https://github.com/Darsain/motio/wiki/Methods#on)找到更多的使用方法。

## 一般性参数

### **this**

在所有回调函数中的**`this`**变量就是触发事件的Motio对象本身。通过这个变量你可以访问到Motio中的所有[属性](https://github.com/Darsain/motio/wiki/Properties)

#### **第一个参数**

所有回调函数接受的第一个参数都是触发事件的名字。

---



例如：

``` javascript
motio.on('frame', function (eventName) {
	console.log(eventName); // 'frame'
	console.log(this.frame); // frame index
});
```


## **事件**

### **暂停 pause**


当动画暂停的时候触发此事件。

### **播放 play**

当动画重新播放的时候触发此事件。

#### **帧切换 frame**

当每一帧动画切换时触发此事件。
