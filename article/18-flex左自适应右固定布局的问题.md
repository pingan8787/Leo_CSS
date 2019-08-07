最近在开发公司项目的时候UI设计稿给了这么一个设计(这里是我手动画的草图)：
![UI设计稿](http://images.pingan8787.com/70-UI%E7%A4%BA%E6%84%8F%E5%9B%BE.png)   
看这效果，第一感觉很简单，flex布局，左边宽度自适应，右边固定宽度。   
先回顾下关于**文本溢出隐藏**的方式：   
```CSS
/* 单行文本 */
.text {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    /*当然还需要加宽度width属来兼容部分浏览。*/
}

/* 多行文本 */
.text {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    overflow: hidden;
    /* -webkit-line-clamp 显示行数 */
}
```

然后开开心心的开始写啊写，代码如下：   
**HTML代码**   
```html
<div id="flex">
    <div id="left">
        <span>这是一个按钮</span>
    </div>
    <div id="right">
        <span>9.9元</span>
    </div>
</div>
```
**CSS样式**   
```CSS
#flex {
    display: flex;
}

#left {
    flex: 1;
}
#left{
    background: red;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
#left span{
    background-color: yellow;
    border-radius: 30px;
    border: 1px solid blue;
    display: inline-block;
}

#right {
    background: green;
    width: 200px;
    margin-left: 10px;
}
```
![UI设计稿2](http://images.pingan8787.com/70-UI%E7%A4%BA%E6%84%8F%E5%9B%BE.png)     

这效果。。有点无语。。右边的圆角去哪里了呢，并且在控制台查看元素，会看到实际`span`元素的宽度非常的宽，且超过父容器`#left`，而`#left`实实在在的还是正常的宽度。   
思考了一会，脑子了出现了各种元素的层叠关系，于是给实际文本内容外面，再添加一层div，来控制内容的宽度。   
**HTML代码**   
```html
<div id="flex">
    <div id="left">
        <div class="box">
            <span>我在左边，自适应布局</span>
        </div>
    </div>
    <div id="right">我在右边，定宽</div>
</div>
```
**CSS样式**   
```CSS
#flex {
    display: flex;
}

#left {
    flex: 1;
    background: red;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
/* 新增的内容的父容器 */
#left .box{
    background: red;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    max-width: 100%;
    border: 1px solid blue;
    border-radius: 100px;
    display: inline-block;
}
#left span{
    background-color: yellow;
    border-radius: 30px;
}

#right {
    background: green;
    width: 200px;
    margin-left: 10px;
}
```
而这里只需把原本设置在`span`上的宽度，边框，圆角和背景色样式，写到父容器`.box`上就可以，`span`里面只负责存放文本内容。  
然后就大功告成了。    
**本文纯属个人看法，欢迎讨论**

* [个人博客](http://www.pingan8787.com)    
* [github](https://github.com/pingan8787)   