> [阅读原文](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb)

## 寻根溯源话布局
一切都始于这样一个问题：怎样通过 `CSS` 简单而优雅的实现水平、垂直同时居中。记得刚开始学习 `CS` 的时候，看到 `float` 属性不由得感觉眼前一亮，顺理成章的联想到 Word 文档排版中用到的的左对齐、右对齐和居中对齐，然而很快就失望的发现 `CSS` 中并不存在 `float: center` 的写法，那么 `text-align: center`、`verticle-align: center` 是否可行呢？答案也是否定的。这两个属性只能用于行内元素，对于块级元素的布局是无效的。  

在网页布局没有进入 `CSS` 的时代，排版几乎是通过 `table` 元素实现的，在 `table` 的单元格里可以方便的使用 `align`、`valign` 来实现水平和垂直方向的对齐，随着 Web 语义化的流行，这些写法逐渐淡出了视野，`CSS` 标准为我们提供了 `3` 种布局方式：`标准文档流`、`浮动布局`和`定位布局`。这几种方式的搭配使用可以轻松搞定 PC 端页面的常见需求，比如实现水平居中可以使用 `margin: 0 auto`，实现水平垂直同时居中可以如下设置：  

```css
.dad {
    position: relative;
}
```
```css
.son {
    position: absolute;
    margin: auto;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
```

```css
.dad {
    position: relative;
}
```
```css
.son {
    width: 100px;
    height: 100px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -50px;
    margin-left: -50px;
}
```
然而，这些写法都存在一些缺陷：缺少语义并且不够灵活。我们需要的是通过 1 个属性就能优雅的实现子元素居中或均匀分布，甚至可以随着窗口缩放自动适应。在这样的需求下，CSS 的第 4 种布局方式诞生了，这就是我们今天要重点介绍的 flex 布局。  

## flex 基本概念
使用 flex 布局首先要设置父容器 `display: flex`，然后再设置 `justify-content: center` 实现水平居中，最后设置 `align-items: center` 实现垂直居中。  
```css
#dad {
    display: flex;
    justify-content: center;
    align-items: center
}
```
![图片1](https://dn-mhke0kuv.qbox.me/933e6f0857399ccf7e83.png?imageslim)  

就是这么简单，大功告成。等等，好像哪里不对，`justify-content` 和` align-items` 是啥？哪里可以看出横向、竖向的语义？是的，`flex` 的确没有那么简单，这就要从两个基本概念说起了。   

![图片2](https://dn-mhke0kuv.qbox.me/221bb6de73e54f4104a1.png?imageslim)  
说来也不难，flex 的核心的概念就是 `容器` 和 `轴`。容器包括外层的` 父容器` 和内层的 `子容器`，轴包括 `主轴` 和 `交叉轴`，可以说 flex 布局的全部特性都构建在这两个概念上。flex 布局涉及到 12 个 CSS 属性（不含 `display: flex`），其中父容器、子容器各 6 个。不过常用的属性只有 4 个，父容器、子容器各 2 个，我们就先从常用的说起吧。  

### 1. 容器
> 容器具有这样的特点：父容器可以统一设置子容器的排列方式，子容器也可以单独设置自身的排列方式，如果两者同时设置，以子容器的设置为准。  
![图片3](https://dn-mhke0kuv.qbox.me/f443b657dbc39d361f68.png?imageslim)  


#### 1.1 父容器
* 设置子容器沿主轴排列：`justify-content`  
`justify-content` 属性用于定义如何沿着主轴方向排列子容器。
![图片4](https://dn-mhke0kuv.qbox.me/be5b7f0e022a8da60ed8.png?imageslim)  

> **flex-start**：起始端对齐  
![图片5](https://dn-mhke0kuv.qbox.me/ac1d8c5e7b4a2ba51ca7.png?imageslim)  

> **flex-end**：末尾段对齐  
![图片6](https://dn-mhke0kuv.qbox.me/9ec9245881c2882a35a6.png?imageslim)  

> **center**：居中对齐  
![图片7](https://dn-mhke0kuv.qbox.me/476461f1b9604a985046.png?imageslim)  
  
> **space-around**：子容器沿主轴均匀分布，位于首尾两端的子容器到父容器的距离是子容器间距的一半。  
![图片8](https://dn-mhke0kuv.qbox.me/63119c88aa64853107a9.png?imageslim)  

> **space-between**：子容器沿主轴均匀分布，位于首尾两端的子容器与父容器相切。  
![图片9](https://dn-mhke0kuv.qbox.me/495f46fc9c5c0c6d1e65.png?imageslim)  

* 设置子容器如何沿交叉轴排列：`align-items`  
`align-items` 属性用于定义如何沿着交叉轴方向分配子容器的间距。    
![图片10](https://dn-mhke0kuv.qbox.me/e7e6aa079f5333828c58.png?imageslim)  

> **flex-start**：起始端对齐 
![图片11](https://dn-mhke0kuv.qbox.me/56622862c7831a4d61be.png?imageslim)   

> **flex-end**：末尾段对齐  
![图片12](https://dn-mhke0kuv.qbox.me/33519955a141be1e713a.png?imageslim)  

> **center**：居中对齐  
![图片13](https://dn-mhke0kuv.qbox.me/f10513a47130d52f2aa8.png?imageslim)  

> **baseline**：基线对齐，这里的 `baseline` 默认是指首行文字，即 `first baseline`，所有子容器向基线对齐，交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线。  
![图片14](https://dn-mhke0kuv.qbox.me/f78e9f42be9a3f165f8f.png?imageslim)  
  
> **stretch**：子容器沿交叉轴方向的尺寸拉伸至与父容器一致。  
![图片15](https://dn-mhke0kuv.qbox.me/160170b3d2022800ffea.png?imageslim)  


#### 1.2 子容器
* 在主轴上如何伸缩：`flex`  
![图片16](https://dn-mhke0kuv.qbox.me/089d48122453e9fc372c.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  
子容器是有弹性的（flex 即弹性），它们会自动填充剩余空间，子容器的伸缩比例由 `flex` 属性确定。  
flex 的值可以是无单位数字（如：1, 2, 3），也可以是有单位数字（如：15px，30px，60px），还可以是 `none` 关键字。子容器会按照 `flex` 定义的尺寸比例自动伸缩，如果取值为 `none` 则不伸缩。  
虽然 flex 是多个属性的缩写，允许 1 - 3 个值连用，但通常用 1 个值就可以满足需求，它的全部写法可参考下图。  
![图片116](https://dn-mhke0kuv.qbox.me/78e9030183f686e0b6ed.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> 单独设置子容器如何沿交叉轴排列：`align-self`
![图片126](https://dn-mhke0kuv.qbox.me/1d09fe5bb413a6dfa5dd.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

每个子容器也可以单独定义沿交叉轴排列的方式，此属性的可选值与父容器 align-items 属性完全一致，如果两者同时设置则以子容器的 align-self 属性为准。

> **flex-start**：起始端对齐  
![图片136](https://dn-mhke0kuv.qbox.me/93d138727b9dd780bdda.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **flex-end**：末尾段对齐  
![图片146](https://dn-mhke0kuv.qbox.me/112f075777fdcb6f5d6f.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  
  
> **center**：居中对齐  
![图片156](https://dn-mhke0kuv.qbox.me/d7b0131447247a5228fe.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **baseline**：基线对齐  
![图片166](https://dn-mhke0kuv.qbox.me/26b04323df92c4b1b023.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **stretch**：拉伸对齐  
![图片176](https://dn-mhke0kuv.qbox.me/ef196e2ba84c406c9ad6.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

### 2. 轴
如图所示，**轴** 包括 **主轴** 和 **交叉轴**，我们知道 `justify-content` 属性决定子容器沿主轴的排列方式，`align-item`s 属性决定子容器沿着交叉轴的排列方式。那么轴本身又是怎样确定的呢？在 flex 布局中，`flex-direction` 属性决定主轴的方向，交叉轴的方向由主轴确定。  
![图片17](https://dn-mhke0kuv.qbox.me/5f2a17efffe8f3ab78a4.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

* 主轴
主轴的起始端由 `flex-start` 表示，末尾段由 `flex-end` 表示。不同的主轴方向对应的起始端、末尾段的位置也不相同。  

> **向右**：flex-direction: row  
![图片18](https://dn-mhke0kuv.qbox.me/da0c2a225cbbdba47297.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **向下**：flex-direction: column  
![图片19](https://dn-mhke0kuv.qbox.me/ab305a50ff35d7e7b6b4.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **向左**：flex-direction: row-reverse  
![图片20](https://dn-mhke0kuv.qbox.me/f3b60f80ddd45974449d.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **向上**：flex-direction: column-reverse  
![图片21](https://dn-mhke0kuv.qbox.me/c219413da157decc5b9e.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

* 交叉轴
主轴沿逆时针方向旋转 90° 就得到了交叉轴，交叉轴的起始端和末尾段也由 `flex-start` 和` flex-end` 表示。  
上面介绍的几项属性是 flex 布局中最常用到的部分，一般来说可以满足大多数需求，如果实现复杂的布局还需要深入了解更多的属性。  



## flex 进阶概念
### 1. 父容器
> 设置换行方式：flex-wrap  
决定子容器是否换行排列，不但可以顺序换行而且支持逆序换行。  
![图片22](https://dn-mhke0kuv.qbox.me/19fb0f3a31fa497191b8.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **nowrap**：不换行  
![图片23](https://dn-mhke0kuv.qbox.me/a41d1342e46cd37cd09e.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **wrap**：换行  
![图片24](https://dn-mhke0kuv.qbox.me/0566bf9682ffa0890624.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **wrap-reverse**：逆序换行  
![图片25](https://dn-mhke0kuv.qbox.me/2f578fcc69919238bd3b.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

逆序换行是指沿着交叉轴的反方向换行。  
![图片26](https://dn-mhke0kuv.qbox.me/2f578fcc69919238bd3b.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  


> 轴向与换行组合设置：`flex-flow  `
flow 即流向，也就是子容器沿着哪个方向流动，流动到终点是否允许换行，比如 `flex-flow: row wrap`，`flex-flow` 是一个复合属性，相当于 `flex-direction` 与 `flex-wrap` 的组合，可选的取值如下：    

* 1. `row`、`column` 等，可单独设置主轴方向  

* 2. `wrap`、`nowrap` 等，可单独设置换行方式  

* 3. `row nowrap`、`column wrap` 等，也可两者同时设置  

> 多行沿交叉轴对齐：`align-content`  
当子容器多行排列时，设置行与行之间的对齐方式。  
![图片27](https://dn-mhke0kuv.qbox.me/ff9bd219375f048b3304.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **flex-start**：起始端对齐  
![图片28](https://dn-mhke0kuv.qbox.me/0183db03d8fedadc4cf8.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **flex-end**：末尾段对齐  
![图片29](https://dn-mhke0kuv.qbox.me/12e524438423ac7afc8c.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **center**：居中对齐  
![图片30](https://dn-mhke0kuv.qbox.me/274a5c1282b997e423db.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **space-around**：等边距均匀分布  
![图片31](https://dn-mhke0kuv.qbox.me/4a435e3fd0cab3433631.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **space-between**：等间距均匀分布  
![图片32](https://dn-mhke0kuv.qbox.me/f50d931bdfeb6c24ccae.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

> **stretch**：拉伸对齐  
![图片33](https://dn-mhke0kuv.qbox.me/878b39463db6bc499fbc.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

### 2. 子容器
* 设置基准大小：`flex-basis`
`flex-basis` 表示在不伸缩的情况下子容器的原始尺寸。主轴为横向时代表宽度，主轴为纵向时代表高度。  
![图片33](https://dn-mhke0kuv.qbox.me/af0dbf4ca6e857ff5de8.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  
![图片33](https://dn-mhke0kuv.qbox.me/7c73d684a32fd8411db6.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

* 设置扩展比例：`flex-grow `   
子容器弹性伸展的比例。如图，剩余空间按 1:2 的比例分配给子容器。 
![图片33](https://dn-mhke0kuv.qbox.me/bcca55b82d18e2ac2367.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  
![图片33](https://dn-mhke0kuv.qbox.me/72e9f508dff25a474b40.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)   

* 设置收缩比例：`flex-shrink `  
子容器弹性收缩的比例。如图，超出的部分按 1:2 的比例从给子容器中减去。  
![图片33](https://dn-mhke0kuv.qbox.me/38596937d4f86beeac0b.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  
![图片33](https://dn-mhke0kuv.qbox.me/d278e36c13b9643ff481.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

* 设置排列顺序：`order`  
改变子容器的排列顺序，覆盖 HTML 代码中的顺序，默认值为 0，可以为负值，数值越小排列越靠前。  
![图片33](https://dn-mhke0kuv.qbox.me/4eb20f9bfc611e66b069.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  

以上就是 flex 布局的全部属性，一共 12 个，父容器、子容器各 6 个，可以随时通过下图进行回顾。  
![图片33](https://dn-mhke0kuv.qbox.me/0dd26d8e99257ff36443.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)  



> 参考资料： 
> * [MDN: CSS Flexible Box Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)  
> * [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  
> * [Flex 布局学习笔记](https://buzheng.org/2017/20170119-flex-layout-note.html)  
> * [30 分钟学会 Flex 布局](https://zhuanlan.zhihu.com/p/25303493)  
> * [弹性盒模型Flex指南](http://louiszhai.github.io/2017/01/13/flex/)  