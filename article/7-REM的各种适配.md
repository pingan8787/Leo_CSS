
## 前言
移动端的崛起，给了我们前端更大的舞台，与此同时，也给我们带来了一系列头疼的问题，移动端适配就是其中之一，目前市面上最常用的方案即是REM适配。  

为什么说她是一个磨人的小妖精？因为她确实让人又爱又恨，灵活的自适应布局再搭配上css单位转换工具，让人爱不释手；另一方面，由于移动端的机型和表现千奇百怪，想要达到完美的兼容又让人头疼。  

即使如此，依然阻止不了笔者对于她的痴迷。本文将会围绕REM适配这一话题进行讨论，同时也会将笔者个人的经验以及自己目前在用的一套代码分享给大家。另外，如今移动端的兼容性越来越好，因此衍生出了一些其他的适配方案，这点不在本文的讨论范围之内。  

## 实例解析
### 全局变量
```js
const docEl = document.documentElement
const metaEl = document.querySelector('meta[name="viewport"]')

const maxWidth = window.__MAX_WIDTH__ || 750
const divPart = window.__DIV_PART__ || 15
const bodySize = window.__BODY_SIZE__ || 12

let scale = 1
let dpr = 1
let timer = null
```

> * metaEl：抓取现有viewport，以支持使用者自定义页面实际缩放比例，通过设置viewport可以实现视觉上的实际物理像素。例如`initial-scale=0.5`，即二倍屏，假设根节点的`font-size=100px`，那么`0.01rem`就是物理像素`1px`；而`initial-scale=1.0`，虽然在css单位中，`0.01rem=1px`，但我们知道，在二倍屏中，1px实际有4个物理像素。  

> * maxWidth：UI稿宽度，一般以iphone6为基准，即750。   

> * divPart：将设备宽度划分为多少份，上述代码中，`750/15=50`，意思是750宽度的屏幕，`1rem=50px`，划分多少份实际上没有固定规定，看个人习惯。   

> * bodySize：初始化时，设置body的字体大小。   

> * scale、dpr分别是页面缩放比例、设备像素比。   


### 初始化设置
```js
if (metaEl) {
  console.warn('根据已有的meta标签来设置缩放比例')

  const match = metaEl.getAttribute('content').match(/initial-scale=([\d.]+)/)

  if (match) {
    scale = parseFloat(match[1])
    dpr = parseInt(1 / scale)
  }
} else {
  if (window.navigator.appVersion.match(/iphone/gi)) {
    dpr = parseInt(window.devicePixelRatio) || 1
    scale = 1 / dpr
  }

  const newMetaEl = document.createElement('meta')
  newMetaEl.setAttribute('name', 'viewport')
  newMetaEl.setAttribute('content', `width=device-width, initial-scale=${scale}, maximum-scale=${scale}, minimum-scale=${scale}, user-scalable=no`)
  docEl.firstElementChild.appendChild(newMetaEl)
}

// 设置根节点dpr
docEl.setAttribute('data-dpr', dpr)
```  

这里要重点将一下为什么要区分安卓和IOS设备，很多人可能会说因为IOS有多倍屏。实际上，安卓也有多倍屏，那为什么我们不考虑呢？   

有些安卓机的设备像素比很奇怪，比如2.5、3.8等一些奇怪的数字；   
部分安卓机表现很奇怪，比如页面宽度比屏幕宽度多一点，出现横向滚动条（具体原因不详，已排除所有css干扰），兼容起来成本太高。   

### 核心代码
```js
function bodyLoaded (cb) {
  if (document.body) {
    cb && cb()
  } else {
    document.addEventListener('DOMContentLoaded', function () {
      cb && cb()
    }, false)
  }
}

// 窗口宽度改变时，刷新rem
function refreshRem () {
  let width = docEl.clientWidth

  if (width / dpr > maxWidth) {
    width = maxWidth * dpr
  }

  // 设置根节点font-size
  window.remUnit = width / divPart
  docEl.style.fontSize = window.remUnit + 'px'

  bodyLoaded(() => {
    // 测试rem的准确性，如果和预期不一样，则进行缩放
    let noEl = document.createElement('div')
    noEl.style.width = '1rem'
    noEl.style.height = '0'
    document.body.appendChild(noEl)

    let rate = noEl.clientWidth / window.remUnit

    if (Math.abs(rate - 1) >= 0.01) {
      docEl.style.fontSize = (window.remUnit / rate) + 'px'
    }

    document.body.removeChild(noEl)
  })
}

// 初始化
refreshRem()

bodyLoaded(() => {
  document.body.style.fontSize = bodySize * dpr + 'px'
  document.body.style.maxWidth = maxWidth * dpr + 'px'
})
```

`refreshRem`函数是整个rem适配的核心，每次需要更新都会调用此函数，我们还限定了页面的最大宽度，可以保证在pc端打开也能看到不错的视觉效果。  
但是有一部分的安卓机，1rem并不等于根节点的font-size，举个例子：html的`font-size=20px`，正常情况下1rem也应该是20px，但在部分机型中，它可能是22px或18px等等（笔者怀疑上文中提到的页面宽度溢出也是这个问题）。因此，笔者加上了bodyLoaded这段代码，在rem设置完成后，再与实际视觉上的1rem进行比较，若偏差超过1%，则认为需要重新定义rem，这样就能100%保证1rem就是我们期望的大小。   

### 页面宽度监听   
```js
window.addEventListener('resize', function () {
  clearTimeout(timer)
  timer = setTimeout(refreshRem, 200)
}, false)

// window.addEventListener('pageshow', function (e) {
//   if (e.persisted) {
//     refreshRem()
//   }
// }, false)
```
这段代码用于监听`resize`事件，以此来重新计算根节点的`font-size`，定时器用来防止频繁计算（实际上在手机中，也不会有频繁触发`resize`的机会，因此定时器也可以不加）。有些读者可能会问题，为什么不监听横竖屏事件（onorientationchange），其实没有必要，横竖屏切换本质也是`resize`的一种，我们已经监听了`resize`事件，这里就没有必要再次监听了。  

那注释掉的这段代码是什么意思呢？它是用来监听浏览器返回，但是这段代码在iPhone8、iPhoneX上会有问题，在返回的时候，我们拿到的`document.documentElement.clientWidth`是其实际的大小（没有乘上设备像素比），因此整个页面布局都乱了。笔者经过深思熟虑，决定删掉这段代码，因为在返回的时候，会保留和离开时一摸一样的状态，没有必要重新再计算一遍。  

### 工具函数
```js
window.px2rem = function (d) {
  let val = parseFloat(d) / window.remUnit

  if (typeof d === 'string' && d.match(/px$/)) {
    val += 'rem'
  }

  return val
}

window.rem2px = function (d) {
  let val = parseFloat(d) * window.remUnit

  if (typeof d === 'string' && d.match(/rem$/)) {
    val += 'px'
  }

  return val
}
```
暴露全局函数，方便使用js来控制尺寸大小。   

### CSS重置样式
篇幅所限，样式代码就不在这里贴了，感兴趣可以在这里看：[reset.css](https://github.com/ansenhuang/axe/blob/master/packages/rem-resize/src/common/reset.css)  

## 总结
这一套rem适配代码是笔者日常开发中总结提炼出来，不能说是100%完美，但是也足够适配市面上的主流机型了。再配合构建工具，自动转换为rem单位，省心又省力。  

最后推荐一个好用的全局构建工具fle-cli，帮你从复杂繁琐的构建配置中解放出来。  

本文源码地址：[github.com/ansenhuang/…](https://github.com/ansenhuang/axe/blob/master/packages/rem-resize/README.md)  

* 阅读原文 [REM，你这磨人的小妖精！](https://juejin.im/post/5b28b36af265da59a36e351e)