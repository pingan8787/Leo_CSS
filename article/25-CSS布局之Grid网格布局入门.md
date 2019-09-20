> [阅读原文](https://mp.weixin.qq.com/s/zPShx9nyeABE36QaJKrXdA)


CSS Grid(网格) 布局使我们能够比以往任何时候都可以更灵活构建和控制自定义网格。Grid(网格) 布局使我们能够将网页分成具有简单属性的行和列。

![25-1.jpg](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-1.jpg)

它还能使我们在不改变任何HTML的情况下，使用 CSS 来定位和调整网格内的每个元素。它允许 HTML 纯粹作为内容的容器。

HTML 结构不再受限于样式表现，比如不要为了实现某种布局而多次嵌套，现在这些都可以让 CSS 来完成。

## 定义一个网格

Grid(网格) 模块为 `display` 属性提供了一个新的值：`grid`。当你将任何元素的 `display` 属性设置为` grid`时，那么这个元素就是一个 网格容器(grid container)，它的所有直接子元素就成了 网格项(grid items)。

让我们创建创建一个` 3×3 `的布局，做一个 Tic-Tac-Toe (井字游戏) 棋盘。首先，我们将写一些 HTML：

HTML 代码：
```html
<div class="game-board">
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
</div>
```

如您所见，`.game-board` div 是网格容器，而 `.box `div 是网格项。现在我们将通过 Grid 布局来实现 `3×3 `布局。

**CSS 代码：**

```css
.game-board 
{
    display: grid;
    grid-template-rows: 200px 200px 200px;
    grid-template-columns: 200px 200px 200px;
}
```

在这里，我还使用了其他两个属性。` grid-template-rows `属性允许我们指定网格中的行数及行的高度。那么你应该猜到另一个属性是干什么的了。

`grid-template-columns` 属性允许我们指定网格中的列数及列的宽度。您可以指定任何单位的尺寸大小，包括像素，百分比和其他单`位fr`，我们将在下一步学习。

## fr 单位(等分)

`fr `是为网格布局定义的一个新单位。它可以帮助你摆脱计算百分比，并将可用空间等分。

例如，如果在网格容器中设置这个规则：`grid-template-rows: 2fr 3fr`，那么你的网格容器将首先被分成 2 行。然后将数字部分加在一起，这里总和为 5， 即 5 等分。

就是说，我们将有 2 行：第一排占据垂直空间的 2/5 。第二排占垂直空间的 3/5 。

回到我们的 Tic-Tac-Toe 例子，我们使用 `fr` 代替 `px`。我们想要的是，应该有3行3列。所以，我们只需要用 3 个 `1fr` 替换 3 个 `200px` 即可：

CSS 代码:

```css
.game-board 
{
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    grid-template-columns: 1fr 1fr 1fr;
}
```

注:

这里特别需要注意的是： `fr` 单位是等分可用空间，或者说剩余空间。看个例子

CSS 代码：
```css
.game-board 
{
    grid-gap:2px;
    display: grid;
    width:300px;
    height:200px;
    grid-template-rows: 100px 1fr 1fr;
    grid-template-columns: 1fr 50px 1fr;
}

```
布局效果如图：

![25-2.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-2.png)


你会看到 `fr` 单位是将 总的尺寸 减去 单元格明确尺寸后，在等分剩余空间。 `grid-gap` 是间隔。

## repeat() 函数

在某些情况下，我们可能有很多的列和行。在 `grid-template` 属性中指定每一个值可能会很乏味。幸运的是，有一个 `repeat` 函数，就像任何一个循环重复多少次输出某个给定值。它有两个参数。第一个是迭代次数，第二个是要重复的值。我们用 `repeat` 函数重写上面的例子。

CSS 代码:
```css
.game-board
{
    display: grid;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);
}
```
等价于：

CSS 代码:
```css
.game-board 
{
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    grid-template-columns: 1fr 1fr 1fr;
}
```

## grid-template 属性

`grid-template` 属性是 `grid-template-rows` 和` grid-template-columns` 的简写语法。这是它的语法：

CSS 代码:
```css
grid-template: ro ws / co lu mns;
```

我们上面的例子使用这个简写语法后，看起来非常整齐。

CSS 代码：
```css
.game-board
{
    display: grid;
    grid-template: repeat(3, 1fr) / repeat(3, 1fr);
}
```

看，只需使用 2 行代码，就可以使用网格布局创建 3×3 网格。

我们在加一些背景和间隔后，上面的例子看起来像这样：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <style>
      .game-board {
        width: 600px;
        height: 600px;
        margin: 0 auto;
        background-color: #34495e;
        color: #fff;
        border: 6px solid #2c3e50;
        border-radius: 10px;
        display: grid;
        grid-template: repeat(3, 1fr) / repeat(3, 1fr);
      }
      
      .box {
        border: 6px solid #2c3e50;
        border-radius: 2px;
        font-family: Helvetica;
        font-weight: bold;
        font-size: 4em;
        display: flex;
        justify-content: center;
        align-items: center;
      }
</style>
  </head>

  <body>
    <div class="game-board">
      <div class="box">X</div>
      <div class="box">O</div>
      <div class="box">O</div>
      <div class="box">O</div>
      <div class="box">X</div>
      <div class="box">O</div>
      <div class="box">O</div>
      <div class="box">X</div>
      <div class="box">X</div>
    </div>
  </body>
</html>
```

效果图：

![25-3.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-3.png)

## 网格线编号,网格单元格，网格轨道

网格线是存在于列和行每一侧的线。一组垂直线将空间垂直划分成列，而另一组水平线将空间水平划分成行。这意味着在我们之前的例子中，有四条垂直线和四条水平线包含它们之间的行和列。


![25-4.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-4.png)


在将网格项从一个位置跨越到另一个位置时，网格线变得非常有用。

网格轨道是两条线之间的空间。网格轨道可以是一行或一列。

网格单元格很像表格单元，是两条相邻垂直线和两条相邻水平线之间的空间。这是网格中最小的单位。

## 定位网格项

我采取了前面的例子的网格，并用数字从1到9标记每个单元格，而不是X或O，下面是它的样子：

![25-5.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-5.png)

假设我想将第 6 个框移到第 2 个框的位置。没有CSS网格，不改变 HTML 的情况下，这几乎是一个不可能的任务，至少对我而言。（注，如果单纯这样的效果图，用FlexBox布局实现问题不大）但是如果我们使用网格模块，改变网格中网格项的位置是一件轻而易举的事情。要将第6个框移到第2个框的位置，我们必须确切知道第2个框在哪里。通过网格线编号的帮助，我们可以很容易地找到这个位置。第二个方框位于第2条列网格线之后，第3条列网格线之前，第1条行网格线之下，第2条行网格线之上。现在我们可以使用以下属性将这些网格线编号分配到第6个框中：

* `grid-column-start`  

* `grid-column-end`  

* `grid-row-start`  

* `grid-row-end`  

前两个属性对应于垂直网格线，也就是列网格线的开始和结束。最后两个属性是指水平网格线，也就是行网格线的开始和结束。让我们分配正确的网格线编号来移动第 6 个框。

CSS 代码:
```css
.box:nth-child(6)
{
    grid-row-start: 1;
    grid-row-end: 2;
    grid-column-start: 2;
    grid-column-end: 3;
}
```

还有两个简写属性用于将行和列的开始网格线和结束网格线设置在一起。

CSS 代码:
```css
.box:nth-child(6)
{
    grid-row: 1 / 2;
    grid-column: 2 / 3;
}
```

此外，还有一个 `grid-area` 属性是所有四个上述属性的简写属性。它按以下顺序取值：

CSS 代码:
```css
grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
```

现在我们的例子可以写成这样



CSS 代码:
```css
.box:nth-child(6)
{
    grid-area: 1 / 2 / 2 / 3;
}
```

上面的代码行也可以进一步减少。正如您所看到的，这个框只占用一行和一个列，所以我们只需要指定行和列的起始线，而无需结束线的值。

CSS 代码:
```css
.box:nth-child(6)
{
    grid-area: 1 / 2;
}
```
如果我们想要第6个框跨越两个框的区域呢？这很容易通过将 `column-end` 值加 1 的办法来完成。

CSS 代码:
```css
.box:nth-child(6)
{
    grid-area: 1 / 2 / 2 / 4;
}
```

![25-6.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-6.png)

您也可以使用 `span` 关键字和占据的 轨道数量，来代替指定 `grid-row-end` 和` grid-column-end `的结束网格线编号。在这种情况下，第6个框是跨越 2 列和 1 行。

CSS 代码:
```css
.box:nth-child(6)
{
    grid-area: 1 / 2 / 2 / span 2;
}
```

## 网格区域命名

`grid-area` 属性也可以用来命名网格的某一个部分，然后我们可以用 `grid-template-areas` 属性来定位。让我们创建一个简单的 `bread-and-butter` 布局，顶部有一个 `top`,` nav`，中间有 `main` 和 `aside`，下面是 `footer`。这是所需的HTML：

HTML 代码:
```html
<div class="container">
  <header></header>
  <nav></nav>
  <main></main>
  <aside></aside>
  <footer></footer>
</div>
```
我们需要使用 `grid-area` 属性来命名每个区域：

CSS 代码:
```css
header
{
  grid-area: header;
  background-color: #9b59b6;
}
 
nav
{
  grid-area: nav;
  background-color: #3498db;
}
 
main
{
  grid-area: main;
  background-color: #2ecc71;
}
 
aside
{
  grid-area: aside;
  background-color: #f1c40f;
}
 
footer
{
  grid-area: footer;
  background-color: #1abc9c;
}
```

现在我们将使用` grid-template-areas `属性来指定每个网格区域所占据的行和列。以下是我们如何做到的：


CSS 代码:
```css
.container
{
      display: grid;
      grid-template-rows: 1fr 5fr 1fr;
      grid-template-columns: 2fr 5fr 3fr;
      grid-template-areas:
        "header  header  header"
        "nav     main    aside"
        "footer  footer  footer";
      grid-gap: .75em;
}
```

请注意，`header` 和 `footer` 单词重复三次。这表明，`header` 和 `footer` 横跨 3 列的宽度。你可以把它全部写在一行中，但是把每一行写在一个单独的行上很好，很干净。你可以看到我在这里使用了一个新的属性 `grid-gap`。它所做的只是在两个网格区域之间添加一个间距。你也可以使用 `grid-row-gap` 和 `grid-column-gap` 来为行和列指定不同的间距值。

CodePen上的这个例子：
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <style>
      .container {
        display: grid;
        grid-template-rows: 1fr 5fr 1fr;
        grid-template-columns: 2fr 5fr 3fr;
        grid-template-areas: "header header header" "nav    main   aside" "footer footer footer";
        grid-gap: .75em;
        background-color: #eee;
        width: 100vw;
        height: 100vh;
      }
      
      header {
        grid-area: header;
        background-color: #9b59b6;
      }
      
      nav {
        grid-area: nav;
        background-color: #3498db;
      }
      
      main {
        grid-area: main;
        background-color: #2ecc71;
      }
      
      aside {
        grid-area: aside;
        background-color: #f1c40f;
      }
      
      footer {
        grid-area: footer;
        background-color: #1abc9c;
      }
</style>
  </head>

  <body>
    <div class="container">
      <header></header>
      <nav></nav>
      <main></main>
      <aside></aside>
      <footer></footer>
    </div>
  </body>

</html>
```
效果图：

![25-7.png](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-7.png)

浏览器支持
在写这篇文章的时候，CSS Grid 布局很多浏览器已经原生支持。根据 caniuse.com 的统计，所有主流浏览器都支持CSS Grid 布局，Internet Explorer 11 部分支持 -ms- 前缀， Opera Mini 根本不支持。

![25-8.jpg](https://github.com/pingan8787/Leo_CSS/blob/master/article/img/25-8.jpg)

## 结论
CSS网格布局允许我们更快地布局，并且更容易控制。在本教程中，我们学习了如何用CSS网格来定义布局，` fr`单位，repeat 函数和一些网格系统中特定的术语。我们还学习了如何使用网格线和网格命名区域在网格容器内定位网格项目。但这只是一个开始。在下一个教程中，我们将深入到CSS网格。


|Author|王平安|
|---|---|
|E-mail|pingan8787@qq.com|
|博  客|www.pingan8787.com|
|微  信|pingan8787|
|每日文章推荐|https://github.com/pingan8787/Leo_Reading/issues|
|ES小册|js.pingan8787.com|

###  微信公众号
![bg](http://images.pingan8787.com/2019_07_12guild_page.png)  