title: Motio-Properties
date: 2014-07-26 17:06:42
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{ config.url }}Motio-Home.html)
[Properties English Version](https://github.com/Darsain/motio/wiki/Properties)
---

Motio对象拥有一些有用的属性。假设：

``` javascript
var motio = new Motio(frame, options);
```
<!--more-->
# **Properties**

### **motio.element**

Type: `Object`

Motio正在操作的页面元素。

### **motio.options**

Type: `Object`

当前在操作的Motio对象的所有属性值。它基本上是在 **`Motio.defaults`**加上初始化Motio **`new Motio()`**时传进去的属性的并集。



### **motio.width**

Type: `Integer`

一帧动画的宽度。它的值不会改变，所以如果你在 **`body`** 元素上设置了panning animation，然后改变这个窗口的大小时也不会影响它的宽度值。不过你也不会在panning模式的动画里面需要用到这个属性。

### **motio.height**

Type: `Integer`

一帧动画的高度。它的值不会改变，所以如果你在 **`body`** 元素上设置了panning animation，然后改变这个窗口的大小时也不会影响它的宽度值。不过你也不会在panning模式的动画里面需要用到这个属性。

### **motio.isPaused**

Type: `Boolean`

当Motio被暂停之后这个属性值便是 **`true`** ，否则为 **`false`** 。

## **Panning mode specific properties**

### **motio.pos**

Type: `Object`

背景位置对象。

``` javascript
{
	x: 100, // Horizontal background position.
	y: 100  // Vertical background position.
}
```

## **Sprite mode specific properties**

### **motio.frame**

Type: `Integer`

当前被激活的帧的index。

### **motio.frames**

Type: `Integer`

当前活动动画一共的帧数。
