title: Motio-Options
date: 2014-07-26 15:41:42
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{config.url}}Motio-Home.html)
[Options English Version](https://github.com/Darsain/motio/wiki/Options)
---

所有的默认值都存放在**`Motio.defaults`** 这个对象里。 你可以方便的设置它的默认值：

``` javascript
Motio.defaults.fps = 60;
```

默认情况下Motio的所有特性都被设置为为启用的。用默认值来初始化一个Motio对象你会得到一个静止的什么都干的动画。
<!--more-->
## **Quick reference**

Motio对象的所有默认值都在下面这段源码中保存：

``` javascript
var panning = new Motio(element, {
  fps:      15, // Frames per second.
	// Sprite animation specific options
  frames:   0, // Number of frames in sprite.
  vertical: 0, // Tells Motio that you are using vertically stacked sprite image.
  width:    0, // Set the frame width manually (optional).
  height:   0, // Set the frame height manually (optional).

  // Panning specific options
  speedX:   0, // Horizontal panning speed in pixels per second.
  speedY:   0, // Vertical panning speed in pixels per second.
  bgWidth:  0, // Width of the background image (optional).
  bgHeight: 0  // Height of the background image (optional).
});
```



# **Options**

---

### **fps**

Type: `Int`
Default: `15`

每秒钟播放几帧动画。 更大的数字意味着更流畅的画面，但需要消耗更多的CPU资源。 最大值是60。

*这个属性可以动态的通过 **`.set()`** 方法来设置。*

## **Sprite animation 模式下特有的属性**

### **frames**

Type: `Integer`
Default: `null`

在一个系列动画图片中，一个动作由几帧自图片完成。设置的这个属性就意味着启用了Sprite Based 模式的动画。否则Motio就处于Panning Based模式。

### **vertical**

Type: `Boolean`
Default: `false`

告诉Motio你在使用纵向的动画子图片，即动作的每一帧是由图片从上往下来进行的。

### **width**

Type: `Integer`
Default: `0`

手动的设置每一帧动画的宽度。这个属性是完全可选的，因为Motio会自动为你计算出这个值，但是如果你要设置Motio动画的这个元素是隐藏的，那Motio就计算不出来了。这样的话，你就得使用这个属性来帮助Motion设置正确的值。

### **height**

Type: `Integer`
Default: `0`

手动的设置每一帧动画的高度。这个属性是完全可选的，因为Motio会自动为你计算出这个值，但是如果你要设置Motio动画的这个元素是隐藏的，那Motio就计算不出来了。这样的话，你就得使用这个属性来帮助Motion设置正确的值。

## **Panning 模式下特有的属性**

### **speedX**

Type: `Integer`
Default: `null`

水平方向上每秒钟动画移动的像素值。使用负值可以是元素倒退。

*这个属性可以动态的通过 **`.set()`** 方法来设置。*

### **speedY**

Type: `Integer`
Default: `null`

垂直方向上每秒钟动画移动的像素值。使用负值可以是元素倒退。

*这个属性可以动态的通过 **`.set()`** 方法来设置。*

### **bgWidth**

Type: `Integer`
Default: `null`

用来平移的背景图片的宽度。这个属性是可选择的。

Motio需要知道这个值和 **`bgHeight`** 这两个属性来计算什么时候将背景图片复位到 **`0`** ，并且也不会溢出Javascript对Integer 2^53的限制。默认不设置的话，它的位置会顺延到一个很可笑的数值，就导致几百万年之后产生一个疯子一样的动画...如果你有强迫症的话你可以去自由设定它们的值。

### **bgHeight**

Type: `Integer`
Default: `null`

用来平移的背景图片的高度。这个属性是可选择的。

## **jQuery plugin options**

这些属性只有在Motio对象是通过jQuery插件来实例化的时候才有效。

### **startPaused**

Type: `Boolean`
Default: `0`

默认的通过jQuery实例化的Motio动画是自动启动的，可以设置这个值为 **`true`** 来阻止它自动启动。
By default, initiating via a jQuery plugin automatically starts the animation. Passing `true` into this value will prohibit that.