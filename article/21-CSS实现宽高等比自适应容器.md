在最近开发移动端页面，遇到这么一个情况：当页面宽度 100% 时，高度为宽度一半，并随手机宽度变化依然是一半。

于是我们就需要实现一个**宽度自适应，高度为宽度一半的容器**。

*这里先以高度为宽度一半为例，也可以是其他任意比例*。

### 一、思考如何实现

这个问题类似于：我们在移动端页面，上面有一张宽度 100% 的图片，如果我们没设置高度，则图片会根据原有尺寸，等比缩放。

我们可以借助这个想法，根据元素高度，来为元素设置一个相应比例的高度即可。

### 二、实现方法1 - 通过 vw 视口单位实现

所谓**视口单位**(viewport units)是相对于视口(viewport)的尺寸而言，`100vw` 等于视口宽度的 `100%`，即 `1vw` 等于视口宽度的 `1%`。

我们就可以利用这个特性，实现移动端的宽高等比自适应容器。

HTML代码：
```html
<div class="box">
    <img src="http://images.pingan8787.com/2019_07_12guild_page.png" />
</div>
```

CSS代码：
```css
*{
    margin:0;
    padding:0
}
.box{
    width:100%;
    height:51.5vw
}
.box img{ 
    width:100%; 
}
```

* **为什么 `.box` 高度为 `51.5vw` 呢？**

原因是图片原来的尺寸是 `884 * 455`的宽高比例，即 `455 / 884 = 51.5%`。

这个方法相比原来图片的等比缩放，有个优点：无论图片是否加载成功，容器高度始终是计算完成，不会造成页面抖动，也不会造成页面重绘，从而提升性能。

下面看看这种情况下，图片加载成功和失败的对比：

![1](http://images.pingan8787.com/20190807css01.png)



### 三、实现方法2 - 通过子元素 padding 实现

通过设置子元素的 `padding` 属性来实现，是比较常用，也是效果比较好的一种，这里需要理解的是：**子元素的 `padding` 属性百分比的值是相对父容器的宽度而言**。

这里看下面代码和效果图理解下：

HTML代码：
```html
<div class="box">
    <div class="text">我是王平安，pingan8787</div>
</div>
```

CSS代码：
```css
.box{
    width: 200px;
}
.text{
    padding: 10%;
}
```

![2](http://images.pingan8787.com/20190807css02.png)

**分析：**


这里我们将父容器 `.box` 宽度设置为 `200px`，子元素 `.text` 的 `padding:10%` ，因此 `.box` 的 `padding` 计算结果为 `20px`;

接下来结合主题，我们利用这个原理，来实现等比例的问题：   

HTML代码：
```html
<div class="box">
    <div class="text">
        <img src="http://images.pingan8787.com/2019_07_12guild_page.png" />
    </div>
</div>
```

CSS代码：

```css
.box{
    width: 100%;
}
.text{
    overflow: hidden;
    height: 0;
    padding-bottom: 51.5%;
}
.box .text img{
    width: 100%;
}
```

这里 `.text` 的 `padding-bottom: 51.5%;` 也是按照第一个方法，用图片原始尺寸的宽高比计算出来的，需要注意，这里将 `.text` 设置 `height: 0;` 会出现高度比实际高的问题，因此为了避免这个情况，就需要设置 `height: 0;`。

于是我们通过2种方法解决了这个问题。   

|Author|王平安|
|---|---|
|E-mail|pingan8787@qq.com|
|博  客|www.pingan8787.com|
|微  信|pingan8787|
|每日文章推荐|https://github.com/pingan8787/Leo_Reading/issues|
|ES小册|js.pingan8787.com|

###  微信公众号
![bg](http://images.pingan8787.com/2019_07_12guild_page.png)  