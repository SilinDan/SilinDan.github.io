---
title: 用简单的css来写一个吃豆人吧
date: 2018-12-23 14:43:13
tags:
categories: 编程 # 分类
thumbnail: https://images.unsplash.com/photo-1551376347-075b0121a65b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=60
---
[我们先来看看将要实现的效果：

  

可以单击这个链接打开查看效果和代码：https://codepen.io/sd237720488/pen/LMyNxJ

 

接下来我们来看看怎么实现的，首先它可以分为两个部分，“吃豆人”和“豆子”

首先HTML部分长这样：](我们先来看看将要实现的效果：

  
![](https://user-gold-cdn.xitu.io/2019/3/7/16956e5c52c72eaa?w=277&h=169&f=png&s=3610)


![](https://user-gold-cdn.xitu.io/2019/3/7/16956e5d84d909da?w=306&h=208&f=png&s=4199)

可以单击这个链接打开查看效果和代码：https://codepen.io/sd237720488/pen/LMyNxJ

 

接下来我们来看看怎么实现的，首先它可以分为两个部分，“吃豆人”和“豆子”

首先HTML部分长这样：

```html

<div class="pac-man">
  //上半圆
  <div class="half">
    <div class="circle"></div>
  </div>
  //下半圆
  <div class="half half-bottom">
    <div class="circle bottom"></div>
  </div>
  //豆子
  <div class="little-circle"></div>
  <div class="little-circle"</div>
  
</div>

```

一、吃豆人部分：

吃豆人由两个半圆组成，先来实现上面的半圆。

首先绘制一个圆

```js
//css部分:
.circle{
  width:50px;
  height:50px;
  border-radius:50%;        
}
```

接着将圆截掉一半，构成上面的半圆：

```css
.half{
  height:25px;
  width:50px;
  overflow:hidden;    
}
```

这样一个半圆就形成了，那么如何让半圆动起来呢？这里用到的是animation。

```css
.half{
    animation:rotate 1s ease infinite;
    transform-origin: 50% 100%;
}

@keyframes rotate{
     50%{
        transform:rotate(-45deg);
    }  
     100%{
        transform:rotate(0deg);
    }  
}

```
下面的圆也差不多是这样实现的。

值得注意的是，下方的圆使用transform:translateY(-50%);将圆向上移动50%，截到的半圆就是下半圆了。

两个圆的transform-origin也是不同的。

另外，动画效果里的-45deg则要变成45deg。

二、豆子部分：

 

首先我们来分析一下，豆子们的起始位置和终点应该是一致的，而速度也应该是相同的。也就是说，所有豆子（虽然我这里只有两颗，但是你们也可以酌情增加豆子数量）路程、时间、速度都一样，那不同的是什么呢？是它们的出发时间

在豆子1还未被吃豆人吃到时，豆子2就应该出发了。倘若豆子1的总运动时间为1秒，豆子2的运动时间也应该为1秒，但是，豆子2应该延迟0.5s，在豆子1运动到一半的时候，豆子2出发。这样就可以造成不断有豆子可以吃的假象了（(*^__^*) 嘻嘻……

```css
.little-circle:nth-of-type(3) {
  animation: walk-left 1s infinite linear;//这里做的是匀速直线运动
}

.little-circle:nth-of-type(4) {
  animation: walk-left 1s infinite linear 0.5s ;
}
```

最后呢，再为豆子的透明度设置变化，出发时是透明的，被吃的时候不透明，造成一个由远到近的假象。

```css
@keyframes walk-left{
  0%{
    opacity:0;
  }
  100%{
    left:15px;
    opacity: 1;
  }
}
```)