```html

<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
        .main{
            width: 400px;
            height: 200px;
            border:1px solid red;
        }
        .box{
            width: 100px;
            height: 100px;
            border:1px solid green;
        }
        .box1{
            margin: 0 auto;
        }
        .textspan{
            text-align: center;
        }
        .textspan2{
            height: 40px;
            line-height: 40px;
            border:1px solid blue;
            text-align: center;
        }
    </style>
</head>
<body>
    <p>居中是我们使用css来布局时常遇到的情况。使用css来进行居中时，有时一个属性就能搞定，有时则需要一定的技巧才能兼容到所有浏览器，本文就居中的一些常用方法做个简单的介绍。</p>
    <p>注：本文所讲方法除了特别说明外，都是兼容IE6+、谷歌、火狐等主流浏览器的。</p>
    <hr>

    <b>1.把margin设置为auto</b>
    <div class="main">
        <div class="box box1"></div>
    </div>

    <b>2、使用 text-align:center</b>
    <div class="main">
        <p class="textspan">这是一段要水平居中的文字。</p>
    </div>

    <b>3、使用line-height让单行的文字垂直居中</b>
    <p>把文字的line-height设为文字父容器的高度，适用于只有一行文字的情况。</p>
    <div class="main">
        <p class="textspan2">这是一段要竖直水平居中的文字。</p>
    </div>
    
    <b>4、使用表格</b>
    <p>td或th可以使用 align="center" 以及 valign="middle"实现水平和垂直居中。</p>
    <p>css控制的话，垂直居中：vertical-align:middle，水平居中：IE6/7可以使用text-align:center，其他浏览器text-align:center只针对行内元素有效</p>
    <div class="main">
        <table>
            <tr>
                <td height="100" width="200" style="border: 1px solid green"; vertical-align:top; text-align:center;>
                    <div style="width: 50px;height: 50px;background-color: red;"></div>
                </td>
            </tr>
        </table>
    </div>

    <b>5、使用display:table-cell来居中</b>
    <div class="main" style="display: table-cell; vertical-align:middle; text-align:center;">
        <div style="width: 50px;height: 50px;background-color: #03f;display: inline-block;"></div>
    </div>
    <p>但是，这种方法只能在IE8+、谷歌、火狐等浏览器上使用，IE6、IE7都无效。</p>

    <b>6、使用绝对定位来进行居中</b>
    <p>此法只适用于那些我们已经知道它们的宽度或高度的元素。</p>
    <p>绝对定位进行居中的原理是通过把这个绝对定位元素的left或top的属性设为50%,这个时候元素并不是居中的，而是比居中的位置向右或向左偏了这个元素宽度或高度的一半的距离，所以需要使用一个负的margin-left或margin-top的值来把它拉回到居中的位置，这个负的margin值就取元素宽度或高度的一半。</p>
    

    <div class="main" style="position: relative;">
        <div style="position: absolute;background-color: #03f;width: 100px;height: 100px;left: 50%;top: 50%;margin-left: -50px;margin-top: -50px;"></div>
    </div>

    <b>7、另一种使用绝对定位来居中的方法</b>
    <p>此法同样只适用于那些我们已经知道它们的宽度或高度的元素，并且遗憾的是它只支持IE9+,谷歌，火狐等符合w3c标准的现代浏览器。</p>
    <div class="main" style="position: relative;">
        <div style="background-color: #03f;position: absolute;width: 100px;height: 100px;left: 0;right: 0;top: 0;bottom: 0;margin: auto;"></div>
    </div>

    <b>8、使用浮动配合相对定位来进行水平居中</b>
    <p>浮动居中的原理是：把浮动元素相对定位到父元素宽度50%的地方，但这个时候元素还不是居中的，而是比居中的那个位置多出了自身一半的宽度，这时就需要他里面的子元素再用一个相对定位，把那多出的自身一半的宽度拉回来，而因为相对定位正是相对于自身来定位的，所以自身一半的宽度只要把left 或 right 设为50%就可以得到了，因而不用知道自身的实际宽度是多少。</p>
    <p>优点是不用知道要居中的元素的宽度，即使这个宽度是不断变化的也行；缺点是需要一个多余的元素来包裹要居中的元素</p>
    <div class="main">
        <div style="position: relative;float: left;left: 50%;clear: both;">
            <div style="border: 1px solid blue;position: relative;left: -50%;white-space: nowrap;">水平居中</div>
        </div>
        <div style="position: relative;float: left;left: 50%;clear: both;">
            <div style="border: 1px solid blue;position: relative;left: -50%;white-space: nowrap;margin-top: 20px">宽度不同</div>
        </div>
        <div style="position: relative;float: left;left: 50%;clear: both;">
            <div style="border: 1px solid blue;position: relative;left: -50%;white-space: nowrap;margin-top: 20px">另一个宽度不同</div>
        </div>
    </div>
</body>
</html>

```