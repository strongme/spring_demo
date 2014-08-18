title: Motio-Markup
date: 2014-07-26 11:31:19
tags:
- Wiki
- Translation
- Motio
categories:
- Translation
---

[Home Chinese]({{config.url}}Motio-Home.html)
[Markup English Version](https://github.com/Darsain/motio/wiki/Markup)
---

Motio通过改变传进来的元素的**`background-position`**来实现动画效果。背景图片可以是一张无缝的整体图片（panning animation）或者也可以是一张有连续动作的静态帧图片（sprite based animation）。

### 例如：

``` html
<div id="panning" class="panning"></div>
<div id="sprite" class="sprite"></div>
```
<!--more-->
背景图片的尺寸可以通过定义样式文件来设置。

``` css
.panning {
	width: auto; // Span to the full width of container
	height: 300px;
	background: url('repeating_sky.jpg');
}

.sprite {
	width: 256px; // Width of one animation frame
	height: 256px; // Height of one animation frame
	background: url('animation_frames_sprite.png');
}
```



然后Motio通过下面的方式来调用动画：

``` javascript
// Panning
var panning = new Motio(document.getElementById('panning'), {
	fps: 30, // Frames per second. More fps = higher CPU load.
	speedX: -30 // Negative horizontal speed = panning to left.
});
panning.play(); // Start playing animation

// Sprite
var sprite = new Motio(document.getElementById('sprite'), {
	fps: 10, // Frames per second. More fps = higher CPU load.
	frames: 14 // Number of animation frames in sprite
});
sprite.play(); // Start playing animation	
```

当设置了**`frames`**属性之后Motio就知道要使用的动画是基于子画面切换的（sprite based）

通过jQuery代理来调用Motio动画是不需要在初始化的时候设置调用**`.play()`** 方法的。
在上面的例子中Motio是在调用**`new Motio`** 实例化之后自动进行的初始化操作。Motio会假设你仅仅是使用jQuery插件来初始化动画然后就没有然后了。即使可以通过jQuery代理来调用方法或者是操作动画，那也看起来没意义啊。已经有了**`new Motio`**，为什么还要去拿到所有元素的代理对象呢？

想知道更多关于方法和属性或者其他所有信息看这里 [RTFM](https://github.com/Darsain/motio/wiki)。

## **平移 Panning**

对于平移模式的动画来说，没有任何的规则，只需要给元素设置一个背景，然后将元素传给Motio就OK了。

## **子画面？ Sprite**（不会翻译哦，求指教）

如果想要一副可以正确播放的Sprite based动画，你必须设置你的背景图片的每一格画面都是等大小的。例如：

<img src="http://i.imgur.com/Sazfe0Q.png">

这就意味着你要放置动画的的宽度和长度，必须和背景图片中的单个子画面的宽度和长度一样一样的。元素的长宽你可以通过样式文件来设置。你可以设置**`padding`**、**`margin`**、**`border`**，只要你设置的**`border-box`**的尺寸和背景图片的单个子画面尺寸一样，Motio就可以照常工作。唯一会导致Motio不正常工作的操作就是改变背景图片的**`background-origin`**属性，所以千万别碰它。

同时调用隐藏起来的元素去执行Motio动画是不会成功的，因为Motio都没办法获取元素的尺寸。这种情况下可以通过设置 [`width`](https://github.com/Darsain/motio/wiki/Options#width) & [`height`](https://github.com/Darsain/motio/wiki/Options#height) 属性来手动进行操作。

