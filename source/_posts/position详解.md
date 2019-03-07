---
title: position详解
date: 2018-12-07 14:49:13
tags:
categories: 编程 # 分类
thumbnail: https://images.unsplash.com/photo-1549396535-c11d5c55b9df?ixlib=rb-1.2.1&auto=format&fit=crop&w=400&q=60
---
# 导读：
position的定位类型有：static(默认值)、relative（相对定位）、absolute（绝对定位）、fixed（固定定位）、sticky（粘性定位）。其中最常用的就是relative和absolute了。我们先来区别一下相对定位和绝对定位，最后再详细讲解其它取值分别是什么意思。

## 相对定位和绝对定位
先看看MDN上是怎么解释它们的：
> 相对定位：相对定位的元素是在文档中的正常位置偏移给定的值，但是不影响其他元素的偏移。

> 绝对定位：相对定位的元素并未脱离文档流，而绝对定位的元素则脱离了文档流。在布置文档流中其它元素时，绝对定位元素不占据空间。绝对定位元素相对于最近的非 static 祖先元素定位。当这样的祖先元素不存在时，则相对于ICB（inital container block, 初始包含块）。

看到这里是不是一脸懵逼呢？大家可以尽量去感悟理解一下，如果实在理解不了也没事，为了更形象地理解relative(相对定位)和absolute(绝对定位)，我们先来看一段代码：

HTML：
```html
<div class="box" id="one">One</div>
<div class="box" id="two">Two</div>
<div class="box" id="three">Three</div>
```

CSS：
```css
.box {
  display: inline-block;
  width: 100px;
  height: 100px;
  background: red;
  color: white;
}

#two {
  position: relative;//相对定位
  top: 20px;
  left: 20px;
  background: blue;
}
```
可以通过Codepen打开尝试：https://codepen.io/sd237720488/pen/oJogNO

## 相对定位 relative
```css
#two {
  position: relative;//相对定位
  top: 20px;
  left: 20px;
  background: blue;
 }
 ```
上面代码用的是相对定位，运行结果如下：
我们可以发现，在相对定位relative的情况下：

> 如果对一个元素进行相对定位，它将出现在它所在的位置上。然后，可以通过设置垂直或水平位置，让这个元素“相对于”它的起点进行移动。——W3school

Two模块：以原本正常位置为参照，向下、向右偏移了20px。
其它元素（One/Three)：均以Two模块正常位置的情况下布局，Two并没有影响其它元素偏移。

## 绝对定位 absolute
当我们把代码中的position：relative替换成position:absolute，会发生什么呢？

```css
#two {
  position: absolute;//相对定位
  top: 20px;
  left: 20px;
  background: blue;
}
```

我们可以发现，在绝对定位absolute的情况下：

> 绝对定位的元素的位置相对于最近的已定位祖先元素，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。——W3school

Two模块：因为没有已定位的祖先元素，所以它的位置相对于最初的包含块，向下、向右偏移了20px。
其它元素（One/Three)：就像Two不存在一样布局。

## 固定定位 fixed
```css
#one {
  position: fixed;
  top: 80px;
  left: 10px;
}
```
"One" 元素固定定位在离页面顶部 80px，离页面左侧 20px 的位置。

## 粘性定位 sticky
```css
#one {
position: -webkit-sticky;
position: sticky; 
top: 10px; 
}
```
以top:10px为临界点，在视图滚动到元素top距离小于10px时,"one"元素为相对定位，超过10px则为固定定位。

sticky为CSS3新增属性。值得注意的是，这个属性的兼容性还不是很好，目前仍是一个试验性的属性，并不是W3C推荐的标准。