## multi-column多列布局实现瀑布流
先简单的讲下multi-column相关的部分属性
* column-count设置列数  
* column-gap设置列与列之间的间距  
* column-width设置每列的宽度  

还要结合在子容器中设置`break-inside`防止多列布局，分页媒体和多区域上下文中的意外中断  
```
break-inside属性值
  auto  指定既不强制也不禁止元素内的页/列中断。
  avoid  指定避免元素内的分页符。
  avoid-page  指定避免元素内的分页符。
  avoid-column 指定避免元素内的列中断。
  avoid-region  指定避免元素内的区域中断。
```
### column属性介绍
1. columns：<' column-width '> || <' column-count '>

设置或检索对象的列数和每列的宽度。复合属性
```css
/*列数及列宽固定*/
-moz-columns:200px 3;
-webkit-columns:200px 3;
columns:200px 3;

/*列宽固定，根据容器宽度液态分布列数*/
-moz-columns:200px;
-webkit-columns:200px;
columns:200px;
```

2. column-width：<' length '> | auto

设置或检索对象每列的宽度；  
auto：根据 <' column-count '> 自定分配宽度  
```css
/*列宽固定，根据容器宽度液态分布列数*/
-moz-column-width:200px;
-webkit-column-width:200px;
column-width:200px;
```

3. column-count：<' integer '> | auto

设置或检索对象的列数；  
auto：根据 <' column-width '> 自定分配宽度  
```css
/*列数固定，根据容器宽度液态分布列宽*/
-moz-column-count:5;
-webkit-column-count:5;
column-count:5;
```

4. column-gap：<' length '> | normal

设置或检索对象的列与列之间的间隙
```css
/*固定列间隙为40px*/
-moz-column-gap:40px;
-webkit-column-gap:40px;
column-gap:40px;

/*列间隙column-gap:normal；font-size为14px时，列间隙column-gap:normal的计算值也为14px*/
-moz-column-gap:normal;
-webkit-column-gap:normal;
column-gap:normal;
```

5. column-rule：<' column-rule-width '> || <' column-rule-style '> || <' column-rule-color '>

设置或检索对象的列与列之间的边框。复合属性  
```css
/*在列与列之间设置绿色间隔线*/
-moz-column-rule:10px solid #090;
-webkit-column-rule:10px solid #090;
column-rule:10px solid #090;
```

6. column-rule-width：<' length '> | thin | medium | thick

medium：定义默认厚度的边框;
thin：定义比默认厚度细的边框;
thick：定义比默认厚度粗的边框

7. column-fill：auto | balance
设置或检索对象所有列的高度是否统一;
auto： 列高度自适应内容;
balance： 所有列的高度以其中最高的一列统一

8. column-break-before：auto | always | avoid | left | right | page | column | avoid-page | avoid-column

设置或检索对象之前是否断行;
auto： 既不强迫也不禁止在元素之前断行并产生新列;
always： 总是在元素之前断行并产生新列
avoid：避免在元素之前断行并产生新列

9. column-break-after：auto | always | avoid | left | right | page | column | avoid-page | avoid-column

设置或检索对象之后是否断行

10. column-break-inside：auto | avoid | avoid-page | avoid-column

设置或检索对象内部是否断行;
auto：既不强迫也不禁止在元素内部断行并产生新列;
avoid：避免在元素内部断行并产生新列

### 演示代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>纯css实现瀑布流（multi-column多列及flex布局）</title>
    <style>
        body {
            background: #e5e5e5;
        }
        
        #root {
            margin: 0 auto;
            width: 1200px;
            column-count: 4;// 设置显示4列
            column-width: 240px;
            column-gap: 20px;//设置或检索对象的列与列之间的间隙为20px
        }
        
        .item {
            margin-bottom: 10px;
            /* 防止多列布局，分页媒体和多区域上下文中的意外中断 */
            break-inside: avoid;
            background: #fff;
        }
        
        .item:hover {
            box-shadow: 2px 2px 2px rgba(0, 0, 0, .5);
        }
        
        .itemImg {
            width: 100%;
            vertical-align: middle;
        }
        
        .userInfo {
            padding: 5px 10px;
        }
        
        .avatar {
            vertical-align: middle;
            width: 30px;
            height: 30px;
            border-radius: 50%;
        }
        
        .username {
            margin-left: 5px;
            text-shadow: 2px 2px 2px rgba(0, 0, 0, .3);
        }
    </style>
</head>

<body>
    <div id="root">
        <div class="item">
            <img src="./img/1.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
        <div class="item">
            <img src="./img/2.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
        <div class="item">
            <img src="./img/3.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
        <div class="item">
            <img src="./img/4.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
        <div class="item">
            <img src="./img/5.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
        <div class="item">
            <img src="./img/6.jpg" alt="" class="itemImg">
            <div class="userInfo">
                <img src="./img/avatar.jpg" alt="" class="avatar">
                <span class="username">景甜大小姐</span>
            </div>
        </div>
    </div>
</body>

</html>
```

### 预览效果
![景甜](http://p3nqtyvgo.bkt.clouddn.com/css_pubuliu.png)