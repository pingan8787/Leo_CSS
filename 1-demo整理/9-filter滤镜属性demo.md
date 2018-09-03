## 参考文档
[文档地址](http://www.runoob.com/cssref/css3-pr-filter.html)

## 实例代码
```html
<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>filter滤镜使用</title>
        <style>
            .box{
                width:300px;
                border: 1px solid red;
                padding: 10px;
                background-color: greenyellow;
                text-align: center;
                margin-bottom: 10px;
            }
            img{
                width: 100%;
                height: 100%;
            }
            .img1{
                filter: blur(10px);
            }
            .img2{
                filter: brightness(60%);
            }
            .img3{
                filter: contrast(50%);
            }
            .img4{
                filter: drop-shadow(5px 5px 10px red);
            }
            .img5{
                filter: grayscale(60%);
            }
            .img6{
                filter: hue-rotate(45deg);
            }
            .img7{
                filter: invert(60%);
            }
            .img8{
                filter: opacity(60%);
            }
            .img9{
                filter: saturate(60%);
            }
            .img10{
                filter: sepia(60%);
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div>原图</div>
            <img src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>blur 高斯模糊</div>
            <img class="img1" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>brightness 亮度调节</div>
            <img class="img2" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>contrast 对比度调节</div>
            <img class="img3" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>drop-shadow 阴影效果调节</div>
            <img class="img4" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>grayscale 黑白比例</div>
            <img class="img5" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>hue-rotate 图像应用色相旋转</div>
            <img class="img6" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>invert 反转输入图像</div>
            <img class="img7" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>opacity 图像透明程度</div>
            <img class="img8" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>saturate 图像饱和度</div>
            <img class="img9" src="./img/img1.jpg" alt="">
        </div>
        <div class="box">
            <div>sepia 图像转换为深褐色比例</div>
            <img class="img10" src="./img/img1.jpg" alt="">
        </div>
    </body>
</html>
```