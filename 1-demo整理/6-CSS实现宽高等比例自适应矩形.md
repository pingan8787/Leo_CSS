本文首发于[虾哔哔的Blog](https://blog.mrcxt.com/2018/05/25/CSS%E5%AE%9E%E7%8E%B0%E5%AE%BD%E9%AB%98%E7%AD%89%E6%AF%94%E4%BE%8B%E8%87%AA%E9%80%82%E5%BA%94%E7%9F%A9%E5%BD%A2/)

## 概述
今天遇到一个很有趣的问题：**如何实现一个宽度自适应，高度为宽度的一半的矩形**。  
经过搜索引擎的筛选和自己的反复试验,发现使用`padding-bottom`是最完美的解决方案。  

## 解决方案
首先我们要明白，padding和margin都是相对于父元素的宽度来计算的，我们可以利用这一属性来实现我们的需求。  
代码如下：  
```html
<div class="scale"></div>
<style>
    .scale {
        width: 100%;
        height: 0;
        padding-bottom: 50%;
    }
</style>
```
这其中的关键点就是`height: 0;`和`padding-bottom: 50%;`。  
我们将元素的高度由`padding`撑开，由于`padding`是根据父元素宽度计算的，所以高度也就变成了相对父元素宽度，同时要将`height`设置为 0，这是为了将元素高度完全交给`padding`负责。  
最后`padding-bottom`的值设为`width`的值一半，就可以实现高度是宽度的一半且自适应啦。  

## 改进
光是这样写还是不够的，因为元素的`height`为 0，导致该元素里面再有子元素的时候，就无法正常设置高度。所以我们需要用到`position: absolute;`。  
代码如下：  
```html
<div class="scale">
    <div class="item">
        这里是所有子元素的容器
    </div>
</div>
<style>
    .scale {
        width: 100%;
        padding-bottom: 56.25%;
        height: 0;
        position: relative; //
    }

    .item {
        width: 100%;
        height: 100%;
        background-color: aquamarine;
        position: absolute; //
    }
</style>
```

## 继续改进
解决了子元素的问题，那么我们再来看看元素本身。由于我们一开始的需求是宽高比 `2:1`,这种比较好实现，但是后来需求又想要 `16:9` 的宽高比，而且宽度不是 `100%`，那这样计算 `padding-bottom` 的时候就很麻烦了。如何解决呢？  
这时候我们需要在外层再套一个父元素，将宽度的控制交给这个父元素来做。  
代码如下：  
```html
<body>
    <div class="box">
        <div class="scale">
            <div class="item">
                item
            </div>
        </div>
    </div>
</body>
<style>
    /* box 用来控制宽度 */
    .box {
        width: 80%;
    }
    /* scale 用来实现宽高等比例 */
    .scale {
        width: 100%;
        padding-bottom: 56.25%;
        height: 0;
        position: relative;
    }
    /* item 用来放置全部的子元素 */
    .item {
        width: 100%;
        height: 100%;
        background-color: aquamarine;
        position: absolute;
    }
</style>
```
如此，就可以完美解决。  

[在线演示](http://jsrun.net/ymZKp/edit)