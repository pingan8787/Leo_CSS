> [阅读原文](https://segmentfault.com/a/1190000019086915)


这是只用了一个 `div` 来做的小动画，纯粹利用 CSS3 的 `animation` 来完成，就像是一个正方形在地上弹跳，碰到地面的时候尖角还会压缩变圆，阴影的部分也会随着正方形升高而缩小，至于到底该怎么完成呢？让我们继续看下去。

## 利用伪元素
由于只使用了一个 `div` ，要同时达到正方形旋转与阴影缩放的效果，这里必须使用两个伪元素（ `before` 与 `after` ）来完成，严格来说，虽然只有一个 `div` ，但是却是把这个 `div` 当作外框，让伪元素 `before` 作为旋转的正方形，让伪元素after作为阴影。

```css
.box{
  position:relative;

}
.box:before{
  content:'';
  position:absolute;
  z-index:2;
  top:60px;
  left:50px;
  width:50px;
  height:50px;
  background:#c00;
  border-radius:2px;
  transform: rotate(45deg);
}
.box:after{
  content:'';
  position:absolute;
  z-index:1;
  top:128px;
  left:52px;
  width:44px;
  height:3px;
  background:#eaeaea;
  border-radius:100%;
}
```

![24-1.jpg](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/24-1.jpg)

## CSS动画

画出正方形与阴影之后，再来就是要做动画了，为了避免太过复杂，这里我们先不要旋转，先单纯让正方形上下跳动，然后阴影会放大缩小，下面的示例的动画，又新增了20%与80%的keyframe，目的是为了让接触的时候角落才会变圆，不然就会变成刚开始移动时尖角就变圆，就会很怪异了。

```css
.box:before{
  content:'';
  position:absolute;
  z-index:2;
  top:60px;
  left:50px;
  width:50px;
  height:50px;
  background:#c00;
  border-radius:2px;
  transform: rotate(45deg);
  -webkit-animation:box .8s infinite ; 
}
@-webkit-keyframes box{
  0%{
    top:50px;
  }
  20%{
    border-radius:2px;  /*从20%的地方才开始变形*/
  }
  50%{
    top:80px; 
    border-bottom-right-radius:25px;
  }
  80%{
    border-radius:2px;  /*到80%的地方恢复原状*/
  }
  100%{
    top:50px;
  }
}
.box:after{
  content:'';
  position:absolute;
  z-index:1;
  top:128px;
  left:52px;
  width:44px;
  height:3px;
  background:#eaeaea;
  border-radius:100%;
  -webkit-animation:shadow .8s infinite ; 
}
@-webkit-keyframes shadow{
  0%,100%{
    left:54px;
    width:40px;
    background:#eaeaea;
  }
  50%{
    top:126px;
    left:50px;   /*让阴影保持在原位*/
    width:50px;
    height:7px;
    background:#eee;
  }
}
```

![24-2.gif](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/24-2.gif)

## 加入旋转效果
了解原理之后，我们只要再加上旋转的属性，就可以达到弹跳起来的时候有旋转的效果，不过这里又有用到一个小技巧，就是落下的时候是90度转到45度，弹上去的时候从45旋转到0度，然后在这一瞬间从0度变成90度（100%到0%），如此一来我们就会产生错觉，感觉好像一直在旋转了。

```css
@-webkit-keyframes box{
  0%{
    top:50px;
    transform: rotate(90deg); /**/
  }
  20%{
    border-radius:2px;
  }
  50%{
    top:80px; 
    transform: rotate(45deg);
    border-bottom-right-radius:25px;
  }
  80%{
    border-radius:2px;
  }
  100%{
    top:50px;
    transform: rotate(0deg);
  }
}
```

![24-3.gif](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/24-3.gif)

举一反三，我们也可以把角度反转，就会往另外一边弹跳。

![24-4.gif](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/24-4.gif)


如果我们把动画里头添加linear，就会变成线性的连续方式喔。

![24-5.gif](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/24-5.gif)


|Author|王平安|
|---|---|
|E-mail|pingan8787@qq.com|
|博  客|www.pingan8787.com|
|微  信|pingan8787|
|每日文章推荐|https://github.com/pingan8787/Leo_Reading/issues|
|ES小册|js.pingan8787.com|

###  微信公众号
![bg](http://images.pingan8787.com/2019_07_12guild_page.png)  