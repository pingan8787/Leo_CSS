前言
一个《别说你懂CSS相对单位》系列文最后一篇了。今日早读文章由阿里@Yuying Wu翻译分享。  
> @Yuying Wu，前端爱好者&鼓励师 / 新西兰working holiday / 铲屎官 / 咖啡爱好者。  

正文从这开始~~  
## 自定义属性（也叫“CSS变量”）  

在2015年，一个大家期待已久的名为“用作层叠式变量的自定义属性”（Custom Properties for Cascading Variables）的CSS规范终于发布为“候选推荐标准”（Candidate Recommendation）。这套规范引入了CSS中“变量”的概念，支持一种新的基于上下文的动态样式定义方式。你可以声明一个变量，再给它赋值，然后就可以在样式表的任何地方引用它。你可以通过这样的方式，减少样式表中的重复代码，以及后续你会看到的一些有用的应用场景。  

在写这本书的时候，自定义属性已经被大多数主流浏览器支持了，除了IE。查看最新的浏览器支持情况，可以查看Can I Use的http://caniuse.com/#feat=css-variables。  

> 笔记
> 如果你刚好在用支持自定义变量的CSS预处理器，如Sass（syntactically awesome stylesheets）或Less，你可能会下意识拒绝CSS变量。千万别这么做。因为原生的CSS变量比任何一个预处理器能实现的功能都要强大和灵活。为了强调它们之间（原生CSS变量和预处理器自定义变量）的差异，我会把它叫作“自定义属性”，而不用“CSS变量”。  

声明一个自定义属性，跟声明其他属性类似。代码片段2.23是自定义属性声明的例子。新建一个页面和样式表吧，然后添加以下的CSS代码。

[ 代码片段 2.23 声明一个自定义属性 ]
```css
:root {
  --main-font: Helvetica, Arial, sans-serif;
}
```
代码片段中，定义了一个名叫—main-font的变量，然后把它的值设定为普通的字体sans-serif。为了和其他属性区分开，命名的前缀必须是两道横杠（—），然后写上你想要的名字。

变量一定要声明在一个声明区块内。在这里，我使用了:root选择器，那么这个变量就可以在整个页面的样式里使用 —— 后面我会简单解释这个问题。

变量的声明，就它本身而言，不会做任何事情，直到我们在代码里引用它。我们在一个段落中使用它吧，做成像图2.13那样的效果。

[ 图 2.13 对一个简单段落使用用变量声明的字体sans-serif ]
![图 2.13](https://mmbiz.qpic.cn/mmbiz_jpg/meG6Vo0MeviaxCJic1Z07PfOw7aEPRsZtsuQHicFIcBChoOMa6LQZzC7V1pnLT3azic8GBeAQXzMicg4rVbhqnr4zeA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

我们可以用一个叫作var()的函数去引用自定义属性的值。现在，你可以利用这个函数去引用我们刚才声明的变量—main-font。把下面展示的代码片段添加到你的样式表中吧，把变量用起来。

[ 代码片段 2.24 使用一个自定义属性 ]
```css
:root {
  --main-font: Helvetica, Arial, sans-serif;
}
p { 
  font-family: var(--main-font);
}
```
* 1 把段落的字体定义为 Helvetica, Arial, sans-serif

自定义属性可以让你在一个地方声明它的值，作为一个“单一数据源”（single source of truth），然后在样式表的任意一个地方引用。这一点对一些反复出现的值特别有用，譬如颜色。下一个代码片段添加了一个名叫brand-color的自定义属性。你可以在样式表中多次使用这个变量，但假如你需要（全局）修改它的值，只需要在一行代码中编辑它的值就可以了。

[ 代码片段 2.25 对color使用自定义属性 ]
```css
:root {
  --main-font: Helvetica, Arial, sans-serif;
  --brand-color: #369;
}
p {
  font-family: var(--main-font);
  color: var(--brand-color);
}
```
* 1 声明一个蓝色的brand-color变量

var()函数支持第二个参数，代表一个默认值。假如一个变量被声明的时候，第一个参数没有被声明，那么第二个参数值就会被引用。

[ 代码片段 2.26 提供回退默认值 ]
```css
:root {
  --main-font: Helvetica, Arial, sans-serif;
  --brand-color: #369;
}
p {
  font-family: var(--main-font, sans-serif); 
  color: var(--secondary-color, blue);
}
```
* 1 声明一个默认值 sans-serif

* 2 变量 secondary-color 没有被声明，于是默认值 blue 会被使用

这段代码在两个不同的声明中，定义了默认值。第一个声明里，—main-font被声明，值为Helvetica, Arial,sans-serif，于是这个值就会被用到了。第二个声明里，—secondary-color是一个没有声明过的变量，所以默认值 blue 被用到了。

> 笔记
> 如果var()被定义为一个无效值，这个属性会被定义为它的初始值。举个例子，如果在padding: var(—brand-color)中，变量是一个色号，那对于padding来说这就是一个无效值。在这个情况下，padding的值会被定义为0。

### 动态改变自定义属性的值

从这些例子可以看到，自定义属性只是更方便了一点，也可以帮助你减少很多的重复代码。但让自定义属性更有意思的是，自定义属性的声明是可以层叠和继承的。你可以在多个选择器中声明同一个变量，这些变量在页面的不同部分可以有着不一样的值。

你可以声明一个变量是黑色的，举个例子，然后在一个特定的容器里把它重新定义为白色的。于是，在这个容器以外的所有依赖这个变量的颜色是黑色，而在容器内的就是白色。通过这样的方式，我们来实现一个像图2.14这样的效果。

[ 图 2.14 自定义属性基于不同域下的值，生成两个颜色不一样的面板 ]
![ 图 2.14](https://mmbiz.qpic.cn/mmbiz_jpg/meG6Vo0MeviaxCJic1Z07PfOw7aEPRsZtsFiaVPzXpgiaDvZl5Esbj6M1AGtYCzX4f9pU063roVTIfFeiaicIVZNEa0g/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

这个面板类似你之前看到的那个（图2.7），HTML在代码片段2.27。这个面板有两个实例，一个在body下，另一个在一个深色的区块。来，更新下你的代码。

[ 代码片段 2.27 页面上不同上下文的两个面板 ]
```html
<body>
  <div class="panel">
    <h2>Single-origin</h2>
    <div class="body">
      We have built partnerships with small farms
      around the world to hand-select beans at the
      peak of season. We then careful roast in
      small batches to maximize their potential.
    </div>
  </div>

  <aside class="dark">
    <div class="panel">
      <h2>Single-origin</h2>
      <div class="body">
        We have built partnerships with small farms
        around the world to hand-select beans at the
        peak of season. We then careful roast in
        small batches to maximize their potential. 
      </div>
    </div>
  </aside>
</body>
```
* 1 页面上一个普通的面板

* 2 第二个面板在深色容器里

我们用变量重新改写一下面板中的文字和背景颜色。把下面的代码片段加进你的样式表。这里把背景颜色设成白色，文字颜色设成黑色。在你添加深色主题之前，我会解释这段代码的工作原理。

[ 代码片段 2.28 利用变量定义面板的颜色 ]
```html
:root {
  --main-bg: #fff; 
  --main-color: #000;
}
.panel {
  font-size: 1rem;
  padding: 1em;
  border: 1px solid #999;
  border-radius: 0.5em;
  background-color: var(--main-bg);
  color: var(--main-color); 
}
.panel > h2 {
  margin-top: 0;
  font-size: 0.8em;
  font-weight: bold;
  text-transform: uppercase;
}
```
* 1 分别把背景色和文字颜色定义为白色和黑色

* 2 在面板样式中使用变量

你再一次把变量声明在:root选择器里。很明显，这样的话我们就可以在根元素（整个页面）下的任何元素中引用这个变量了。当根元素下的子元素使用这些变量时，它们就能拿到这些变量对应的值。

你有两个面板，不过它们仍然看起来是一样的。现在，再一次定义这些变量，但这次是在一个不同的选择器中。下一个代码片段是深色容器的，它有深灰色的背景色，以及小小的padding和margin。同时，它也重写了两个变量。添加到你的样式表吧。

[ 代码片段 2.29 设置深色容器的样式 ]
```css
.dark {
  margin-top: 2em; 
  padding: 1em;
  background-color: #999; 
  --main-bg: #333;      
  --main-color: #fff;  
}
```
* 1 在深色容器和上一个容器间设定一个margin

* 2 给深色容器设定深灰色的背景色

* 3 在当前容器的作用域下，重新定义–main-bg 和 –main-color的值

刷新页面，第二个面板就会有深色背景和白色文字。这是因为当这个面板去调用这些变量时，拿到的是深色容器作用域下的值，而不是根元素域下的值。注意，你并不需要修改这个容器里的样式或者添加额外的类名。

在这个例子里，你两次定义了自定义属性，第一次在根元素作用域上（—main-color是黑色的），第二次在深色容器作用域（—main-color是白色的）。自定义属性表现得像作用域变量，因为值会被后代元素继承。在深色容器中，—main-color是白色的，而在页面的其他位置，它是黑色的。
### 通过JavaScript改变自定义属性的值

在浏览器中，自定义属性还可以被JavaScript访问和动态地修改。毕竟这不是一本讲JavaScript的书，我会告诉你足够多的基本概念，然后你再把这些融入到自己的JavaScript项目中。

[ 代码片段 2.30 在JavaScript里访问一个自定义变量 ]
```html
<script type="text/javascript">
  var rootElement = document.documentElement;
  var styles = getComputedStyle(rootElement);
  var mainColor = styles.getPropertyValue('--main-bg'); 
  console.log(String(mainColor).trim());
</script>
```
* 1 获取元素的样式对象（style object）

* 2 从样式对象中获得 –main-bg 的值

* 3 确认 mainColor 是一个字符串以及把空格去掉，输出“#fff”

因为你可以随手修改自定义属性的值，你可以用JavaScript给—main-bg动态地定义一个新的值。如果你把它定义为浅蓝色，它就是展示成这样（图2.15）。

[ 图 2.15 JavaScript可以通过改变变量–main-bg的值改变面板的背景色 ]
![图 2.15](https://mmbiz.qpic.cn/mmbiz_jpg/meG6Vo0MeviaxCJic1Z07PfOw7aEPRsZtsvDyibicia4t3oMl6DibYb4TG6Pyzamvj8deOaeWCFOA7AlS4hCSt8cXktg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

下面的代码片段，会在根元素下给—main-bg定义一个新的值，在`<script>`标签的最下面，加上这些的代码。

[ 代码片段 2.31 在JavaScript定义一个自定义变量的值 ]
```javascript
var rootElement = document.documentElement;
rootElement.style.setProperty('--main-bg', '#cdf');
```
* 1 把根元素下的 –main-bg 定义为浅蓝色

如果你执行这段代码，任何继承了—main-bg属性的元素都会发生改变，对应的值会变成新的。在你的页面上，这会把第一个面板的背景色变成浅蓝色。第二个面板保持不变，因为它继承的还是在深色容器里定义的值。

利用这项技术，你可以在浏览器里用JavaScript给你的站点换主题。或者你可以高亮页面上的某些部分，又或者随手就可以做一些改变。只需要少量几行JavaScript代码，你做的改变就可以影响到页面上大量的元素。
### 初探自定义属性

自定义属性是一个全新的CSS领域，开发者才刚刚开始探索。因为目前浏览器的支持比较有限，所以还没有到使用它的“黄金时间”。我相信，一段时间之后，你会看到很多关于自定义属性的最佳实践和新颖的玩法。这是你需要留意的。尝试使用自定义属性，看看你可以做出些什么吧。

需要关注的一点，如果你使用`var()`声明，低版本浏览器不能识别就会忽略它。如果可以的话，给那些浏览器提供一个回退（fallback）方案。

[ 代码片段（没有编号） ]
```css
color: black;
color: var(--main-color);
```
自定义属性原生的动态特性，并不是总是可以使用的，可以关注它的[浏览器支持情况](http://caniuse.com)。
## 总结

* 拥抱和使用相对单位，让页面的结构去定义样式代码的含义

* 个人喜欢对字号大小使用rem，选择性地对页面组件的一些简单缩放效果使用em

* 你可以让整个页面实现响应式缩放，而不需要任何的媒体查询

* 在声明行高时，使用不带单位的数值

* 开始了解和使用CSS最新的特性之一——自定义属性吧！

关于本文
译者：@Yuying Wu
译文：

作者：@Keith J.Grant
原文：
https://livebook.manning.com/#!/book/css-in-depth/chapter-2
