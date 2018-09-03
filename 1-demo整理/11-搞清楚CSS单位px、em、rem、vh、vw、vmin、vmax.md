### 1.px
相对长度单位，像素px是相对于显示器屏幕分辨率而言的。

### 2.em
相对长度单位，相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。 
```html
<body>body
    <div class="div1">div1
        <div class="div2">div2
            <div class="div3">div3</div>
        </div>
    </div>
</body>
```
```css
div{
    font-size:1.5em;
}
```
![预览](http://p3nqtyvgo.bkt.clouddn.com/a1.png)  

计算关系是这样的：  
```
body的font-size是继承自跟元素html，html的尺寸是浏览器默认尺寸14px；
div1的font-size=1.5*14px = 21px;
div2的font-size=1.5*21px = 31.5px;
div3的font-size=1.5*31.5px = 47.25px;
```
如果手动设置div2的font-size为40px，div3的font-size应该为1.5*40px = 60px。

### 3.rem
相对长度单位。r 是`root`的缩写，相对于根元素<html>的字体大小。  
例如还是上面的html代码，添加如下样式：  
```css
.div3{
    font-size:1.5rem;
}
```
![预览](http://p3nqtyvgo.bkt.clouddn.com/a2.png)   

此时div3的font-size = 1.5*14px = 1.5*html的font-size。  

另外需注意chrome强制最小字体为12号，即使设置成 10px 最终都会显示成 12px，当把html的font-size设置成10px,子节点rem的计算还是以12px为基准，所以网上很多文章提到的将html的font-size设为10方便计算不是那么可取。   
rem在移动端应用可参考淘宝的页面http://m.taobao.com (html的font-size通过动态计算获取)  
页面基准320px(20px),html font-size值的计算：  
```js
var ele=document.getElementsByTagName("html")[0],
size=document.body.clientWidth/320*20;
ele.style.fontSize=size+"px"
```
注：需设置meta缩放比1:1  
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />
```

### 4.vh and vw：
相对于视口的高度和宽度，而不是父元素的（CSS百分比是相对于包含它的最近的父元素的高度和宽度）。  

1vh 等于1/100的视口高度，1vw 等于1/100的视口宽度。  

比如：浏览器高度900px，宽度为750px, 1 vh = 900px/100 = 9 px，1vw = 750px/100 = 7.5 px。  

很容易实现与同屏幕等高的框：`.slide { height: 100vh;}`  
设置一个和屏幕同宽的标题，`h1{font-size:100vw}`，那标题的字体大小就会自动根据浏览器的宽度进行缩放，以达到字体和viewport大小同步的效果。  

### 5.vmin and vmax：
关于视口高度和宽度两者的最小值或者最大值。  
比如，浏览器的宽度设置为1200px，高度设置为800px， 1vmax = 1200/100px = 12px， 1vmin = 800/100px = 8px。如果宽度设置为600px,高度设置为1080px, 1vmin就等于6px, 1vmax则未10.8px。  

有一个元素，你需要让它始终在屏幕上可见：  
```css
.box { 
    height: 100vmin; 
    width: 100vmin;
}
```
![预览](http://p3nqtyvgo.bkt.clouddn.com/a3.png)  

如果你要让这个元素始终铺满整个视口的可见区域：  
```css
.box { 
    height: 100vmax; 
    width: 100vmax;
}
```
![预览](http://p3nqtyvgo.bkt.clouddn.com/a4.png)  


### 注意：
vw, vh, vmin, vmax：IE9+局部支持；  
chrome/firefox/safari/opera支持；  
ios safari 8+支持；  
android browser4.4+支持；  
chrome for android39支持；