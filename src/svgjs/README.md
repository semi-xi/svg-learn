# SVG.js 中文说明

## 说明
网上好像没有相关的中文文档，这样的话我就自己写一个了，按照官方那边的来写

[SVG.js官网GitHub](https://github.com/wout/svg.js 'svg.js');

支持IE9+

另外这个翻译仅仅只是个人学习交流之用啊，千万别作其他用途，另外很多都是自己的理解，看不懂的话可以去看下源文。小白一个就不要吐槽那么多了


## 用法
### Create an SVG document 创建一个SVG的节点
使用`SVG()`方法来创建一个svg的节点到给定的一个html节点之中

```javascript
var draw = SVG('drawing').size(300, 300)
var rect = draw.rect(100, 100).attr({ fill: '#f06' })

```
第1个参数既可以是一个给定id的节点，也可以是一个已经被获取了的节点，这样会被生成如下的结构

```html
<div id="drawing">
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" height="300">
    <rect width="100" height="100" fill="#f06"></rect>
  </svg>
</div>
```

自己注： 其中，id为`drawing`的div是原本就存在的了，这样的话会把svg插入到这个节点之内

在默认的情况下，svg的大小取决于你第1个参数的尺寸大小，就相当于
```javascript
var draw = SVG('drawing').size('100%', '100%')
```

### Checking for SVG support 检查是否支持svg
在使用SVG.js的时候，它是默认你支持svg的，但是你可以使用它提供的方法去检测是否支持svg

```javascript
if (SVG.supported) {
  var draw = SVG('drawing')
  var rect = draw.rect(100, 100)
} else {
  alert('SVG not supported')
}
```

### SVG document SVG节点
SVG.js在html节点之外，在svg节点里面也能够可以正常的运行，例如

```html
<?xml version="1.0" encoding="utf-8" ?>
<svg id="drawing" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" >
  <script type="text/javascript" xlink:href="svg.min.js"></script>
  <script type="text/javascript">
    <![CDATA[
      var draw = SVG('drawing')
      draw.rect(100,100).animate().fill('#f03').move(100,100)
    ]]>
  </script>
</svg>
```

### Sub-pixel offset fix 子元素的偏移
使用方法`spof()`来获取子元素的偏移

```javascript
var draw = SVG('drawing').spof()
```

当改变窗口大小要使之自适应偏移的时候，可以使用下面的方法

```javascript
SVG.on(window, 'resize', function() { draw.spof() })
```

自己注: 暂时不上很懂这个是什么意思,自己试了一下，如果给svg加定位的话用了这个方法，他会自动的修改使之回到`left:0,top:0`

## 父节点

### Main svg document SVG根节点

SVG.js主要的初始化方法是在一个给定的节点中创建一个svg的根节点标签，并且返回一个`SVG.Doc`实例

```javascript
var draw = SVG('drawing')
```
`return`: `SVG.Doc`

继承关系 `SVG.Doc` < `SVG.Container` < `SVG.Parent`

### Nested SVG 嵌套svg

利用这个方法，你可以嵌套在任意一个节点之中。这个嵌套的的svg所拥有的特性方法跟顶层svg节点是一样的。

`return`: `SVG.Nested`

继承关系 `SVG.Nested` < `SVG.Container` < `SVG.Parent`

### Groups 分组

如果你想transform一组元素，那么对元素分组是非常有用的。所有的节点都在一个分组里面，这样能很好的保证他们所属的相对位置，一个`group`能像svg根节点那样，拥有所有的调用节点方法

```javascript
var group = draw.group()
group.path('M10,20L30,40')
```

如果一个节点已经存在的话，也可以把它增加进去一个`group`里面
`group.add(rect)`

注意： `groups`是没有大小的，它依赖于它的内容。因此它是没有`x`,`y`,`width`跟`height`,如果你需要这些的话，使用`nested()`,嵌套svg去代替`group`

`return`: `SVG.G`

继承关系 `SVG.G` < `SVG.Container` < `SVG.Parent`


### Hyperlink 超链接
一个超链接或者`<a>`标签创建的内容可以使得它的所有子级都是指向一个链接

```javascript
var link = draw.link('http://svgjs.com')
var rect = link.rect(100, 100)
```

链接可以用`to()`去更新

```javascript
link.to('http://apple.com')
```

此外，这个link的节点还有一个`show()`的方法去设置`xlink:show`的属性

```javascript
link.show('replace')
```

然后还有一个`target()`的方法去改变`target`的属性

```javascript
link.target('_blank')
```

节点还可以用`linkTo()`，让自己链接到其他地方去

```javascript
rect.linkTo('http://svgjs.com')
```

或者可以通过一个块去代替一个url来获取这个链接节点的更多参数

```javascript
rect.linkTo(function(link) {
  link.to('http://svgjs.com').target('_blank')
})
```

`return`: `SVG.A`

继承关系 `SVG.A` < `SVG.Container` < `SVG.Parent`


### Defs 引用

`<defs>`节点是一个放置引用元素的容器。它的所有后代元素都不会被渲染。`<defs>`会一直存在于svg节点中，并且可以利用`defs()`来进行存取。

```
var defs = draw.defs()
```

defs还可以通过另一个元素的`doc()`方法获取到

```javascript
var defs = rect.doc().defs()
```

`return`: `SVG.A`

继承关系 `SVG.A` < `SVG.Container` < `SVG.Parent`

## Rect 矩形

Rect 有2个参数，他们分别是`width`和`height`

```javascript
var rect = draw.rect(100, 100)
```

`return`: `SVG.Rect`

继承关系 `SVG.Rect` < `SVG.Shape` < `SVG.Element`


### radius()

矩形还可以设置他们的弧度

```javascript
rect.radius(10)
```

这样的会设置他们的`rx`和`ry`都是10，如果要分别设置的话，可以向下面这样

```javascript
rect.radius(10, 20)
```

`returns`: `itself`

## Circle 圆

它只有一个参数，这个参数是它的直径

```javascript
var circle = draw.circle(100)
```

`return`: `SVG.Circle`

继承关系 `SVG.Circle` < `SVG.Shape` < `SVG.Element`

### radius()

重新定义它的半径

```javascript
circle.radius(75)
```

`returns`: `itself`

## Ellipse 椭圆

跟矩形一样，有两个参数`width`,`height`

```javascript
var ellipse = draw.ellipse(200, 100)
```

`return`: `SVG.Ellipse`

继承关系 `SVG.Ellipse` < `SVG.Shape` < `SVG.Element`

### radius()

允许重新设置两个半径

```javascript
ellipse.radius(75, 50)
```

`returns`: `itself`


## Line 直线

创建一条从点a到点b的直线

```javascript
var line = draw.line(0, 0, 100, 150).stroke({ width: 1 })
```

生成了的直线改变它有4种方式，可以看下`plot()`方法查阅所有的用法

### plot()

更新一个直线可以用`plot()`来更新

```javascript
line.plot(50, 30, 100, 150)
```

或者它也接受传入的参数是一个坐标字符串

```javascript
line.plot('0,0 100,150')
```

或者参数是一个数组

```javascript
line.plot([[0, 0], [100, 150]])
```

也可以用`SVG.PointArray`代替

```javascript
var array = new SVG.PointArray([[0, 0], [100, 150]])
line.plot(array)
```
`returns`: `itself`

### array() 坐标点

参考于 `SVG.PointArray`的实例，这个方法只能限于内部使用  
方法的作用返回是一个数组，数组的每一个值都是包含x,y的点

```js
polyline.array()
```

`returns`: `SVG.PointArray`



## Polyline 折线

折线它定义一组连接在一起的直线,它的形状不是闭合在一起的，也就是起点跟终点不是不会直接连接

```javascript
var polyline = draw.polyline('0,0 100,50 50,100').fill('none').stroke({ width: 1 })
```

折线他是由一组用空格分割的点组成的，就像这样`x,y x,y x,y`
如果你用一组数组的话，也是同样可以绘制出的，就像下面这样

```javascript
// polyline([[x,y], [x,y], [x,y]])
var polyline = draw.polyline([[0,0], [100,50], [50,100]]).fill('none').stroke({ width: 1 })

```

`returns`: `SVG.Polyline`

继承关系 `SVG.Polyline` < `SVG.Shape` < `SVG.Element`

### plot()

polyline也可以用`plot()`这个方法来更新点

```javascript
polyline.plot([[0,0], [100,50], [50,100], [150,50], [200,50]])

```

`plot()`这个方法还可以结合`animate`做成动画

```javascript
polyline.animate(3000).plot([[0,0], [100,50], [50,100], [150,50], [200,50], [250,100], [300,50], [350,50]])
```

`returns`: `itself`

### array()

因为`polyline`是`SVG.PointArray`的实例，当然也可以使用它的内部方法

`returns`: `SVG.Polyline`

## Polygon 多边形

polygon(多边形)它不像polyline(折线)那样，它是由一组前后连接的线段组成的一个封闭图形，就像这样 :

```javascript
// polygon('x,y x,y x,y')
var polygon = draw.polygon('0,0 100,50 50,100').fill('none').stroke({ width: 1 })
```

polygon对于参数的要求跟polyline是一样的，这里没有必要让最后一个点跟第1个点一样来达到闭合的目的，它的闭合是自动的，自动将第1个点跟最后一个点连接起来

`returns`: `SVG.Polygon`

继承关系 `SVG.Polygon` < `SVG.Shape` < `SVG.Element`


### plot()

polygon也可以用`plot()`这个方法来更新点

```javascript
polygon.plot([[0,0], [100,50], [50,100], [150,50], [200,50]])
```

`plot()`这个方法还可以结合`animate`做成动画

```javascript
polygon.animate(3000).plot([[0,0], [100,50], [50,100], [150,50], [200,50], [250,100], [300,50], [350,50]])
```

`returns`: `itself`

### array()

因为`polygon`是`SVG.PointArray`的实例，当然也可以使用它的内部方法

`returns`: `SVG.PointArray`


## Path 路径

路径描绘出来的线跟多边形是很像的，但是它为了能支持曲线会显得比较的复杂

```javascript
draw.path('M 100 200 C 200 100 300  0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
```

`returns`: `SVG.Path`

继承关系 `SVG.Path` < `SVG.Shape` < `SVG.Element`

关于`path`更详细的信息，可以参考SVG Documentation ：
['http://www.w3.org/TR/SVG/paths.html#PathData']('http://www.w3.org/TR/SVG/paths.html#PathData',pathdata)


### plot()

path也可以用`plot()`这个方法来更新点

```javascript
path.plot('M100,200L300,400')
```

`returns`: `itself`

### array()

因为`path`是`SVG.PointArray`的实例，当然也可以使用它的内部方法

`returns`: `SVG.PointArray`

## Image 图片

创建图像是你所期待的

```javascript
var image = draw.image('/path/to/image.jpg')
```

如果你知道图片的尺寸的话，可以通过第2，3个参数传递进去

```javascript
var image = draw.image('/path/to/image.jpg', 200, 300)
```

`returns`: `SVG.Image`

继承关系 `SVG.Image` < `SVG.Shape` < `SVG.Element`


### load()

你可以用`load()`方法来加载另外一张图片

```javascript
image.load('/path/to/another/image.jpg')
```


### loaded()

如果你不知道图片的尺寸，很显然你需要等图片加载完

```javascript
var image = draw.image('/path/to/image.jpg').loaded(function(loader) {
  this.size(loader.width, loader.height)
})
```

返回值loader对象里面包含4个值：
1. `width`
1. `height`
1. `ratio` (width/height)
1. `url`

`return` : `itself`

## Text 文本

与HTML的文本不同，文本在SVG是比较难控制的。没有办法去创建随心所欲的文本，所以如果文本要另起一行的话，就需要手动的加入换行符。在SVG有2种办法创建SVG文本  

首先，也是最简单的办法是提供一个文本字符串，可以加入你所需要的换行符去换行

```javascript  
var text = draw.text("Lorem ipsum dolor sit amet consectetur.\nCras sodales imperdiet auctor.")
```

这样会创建出一个文本块，并且在需要的地方换行  

第2个方法会让你更加自由的控制文本，但是这样也需要更多的代码：

```javascript
var text = draw.text(function(add) {
  add.tspan('Lorem ipsum dolor sit amet ').newLine()
  add.tspan('consectetur').fill('#f06')
  add.tspan('.').newLine()
  add.tspan('Cras sodales imperdiet auctor.').newLine().dx(20)
  add.tspan('Nunc ultrices lectus at erat').newLine()
  add.tspan('dictum pharetra elementum ante').newLine()
})
```

如果你想要另外一种方法，并且不想总是创建`tspan`，并且只仅仅只有一行，那样你可以使用`plain()`这个办法去代替:  
(自己注：这样的话就是直接创建的是一个`text`标签而不是`tspan`标签)  

```javascript
var text = draw.plain('Lorem ipsum dolor sit amet consectetur.')
```

当你不需要换行的时候，`SVG.Text`的`plain`方法是一个比较方便快捷的方法

继承关系 `SVG.Text` < `SVG.Shape` < `SVG.Element`

### text()

改变文本的内容可以通过`text()`方法来实现

```javascript
text.text()
```

`returns`: `itself`

获取全部的文本也可以使用`text()`方法

```javascript
text.text()
```

`returns`: `string`

### tspan()

增加一个tspan

```javascript
text.tspan(' on a train...').fill('#f06')
```

### plain()

如果该元素不需要多栏或者换行，那样这个方法就足够可以去增加一些需要的文本了。

```javascript
text.plain('I do not have any expectations.')
```

`returns`: `itself`

### font()

`sugar.js`模块提供了一些特别的语法给这些节点

```javascript
text.font({
  family:   'Helvetica'
, size:     144
, anchor:   'middle'
, leading:  '1.5em'
})
```

`returns`: `itself`

### leading() 间距

对于HTML来说，间距是通过`line-height`去定义的，在SVG中并有像HTML那样有样式可以去定义行高。在SVG中，基线是没有被定义出来的。他们都是通过`tspan`的一个`dy`属性去定义这个行高，一个`x`值去重新设置基线在父元素`text`的水平位置。但是你也可以有很多节点在一条基线上，但是拥有不同的`y` 、`dy` 、 `x` 甚至是 `dx`值。这给了我们更多的自由，但是也给了我们更多的麻烦。我们应该决定一个基线应该什么时候去定义，它的起始点是哪里，偏移量是多少，高度是多少。当你设置一些样式行为时，SVG的`leading()`方法尽可能的接近HTML。文本的多行组合，它看了来就跟HTMl跟接近了。

```javascript
var text = draw.text("Lorem ipsum dolor sit amet consectetur.\nCras sodales imperdiet auctor.")
text.leading(1.3)
```

这样会为`text`会在渲染每一行`tspan`的时候，为它们的dy属性设置`130%`的`font-size`大小

需要注意的是，leading()方法假设一个文本节点中的每一级`tspan`都代表新的一行。如果是使用在一个包含很多`tspan`的文本节点中（没有通过包裹`tspan`来定义新的一行），会被渲染成不正常的，所以最好小心的使用这个方法，最好是用在用换行符分割的文本或者是调用`newline()`方法，通过传递参数，去设置每一级的`tspan`，使之成为块级。


PS ：就是用的时候最好保证他是一行一行的，而不是连续的span，这样的话会有问题，我是这么理解的

`return` : `itself`


### build()

`build()`方法可以被用于设置启用或者禁用绘制模式。当绘制式被禁用的时候，`plain()`方法跟`tspan()`方法都会在增加新文本之前先调用`clear()`方法。当绘制模式被启用的时候，`plain()`跟`tspan()`方法会先增加新的文本内容到已有的内容之中。当遇到一个`text()`方法创建的文本块时，绘制模式会自动在这个文本块之前或者之后调用，但是在某些情况下，它可以让自由的手动切换。

ps: 类似于锁住当前文本内容，在内容中插入新的内容，否则每次调用都会先把之前的都清除掉。

```js
var text = draw.text('This is just the start, ')

text.build(true)  // enables build mode

var tspan = text.tspan('something pink in the middle ').fill('#00ff97')
text.plain('and again boring at the end.')

text.build(false) // disables build mode

//text.plain('4444')  如果之前build不是true的话这里执行了会清除之前的所有文本

tspan.animate('2s').fill('#f06')
```


### rebuild()

这是一个内部的回调，可能永远都不需要手动去调用这个方法。基本上当这个文本节点`font-size`,`x`属性或者调用`leading()` 去更新这个节点时，就会发生重绘。这个方法也可以设置或者禁用掉绘制

```js
text.rebuild(false) //-> disables rebuilding
text.rebuild(true)  //-> enables rebuilding and instantaneously rebuilds the text element  使之可以发生重绘并且马上重绘当前的文本节点
```

`return` : `itself`


### clear()

清除当前文本里的所有内容
PS：类似于html元素的`innerHTML = ''`；

```js
text.clear()
```

`return` : `itself`

### length()

获得计算之后文本的长度

PS：这里获取出来的是文字所占的内容宽度，而不是文字的长度，两者是有区别的，
其内部使用的是`getComputedTextLength`来获取的。

`return` : `number`

### lines()

获取文本标签的一级`tspan`节点。返回的是一个包含`members`的对象，对象里面是一个包含各个`tspan`的数组
PS： 如果是用`plain()`创建的话，则返回的是空数组

`return` : `SVG.Set`

### events

文本标签只有一个事件，它会在每次`rebuild()`的时候触发这个回调

```js
text.on('rebuild', function() {
  // whatever you need to do after rebuilding
  //
})
```

## Tspan

`tspan`标签只有在`text`标签或者在其他`tspan`标签才会有效，在SVG.js里面，他们有自己专属的类。

继承关系 `SVG.Tspan` < `SVG.Shape` < `SVG.Element`

### text()

可以通过一个字符串的参数来更新`tspan`的内容。

```js
tspan.text('Just a string.')

```

它这里调用的是plain的方法去实现的
或者也可以通过一个回调函数，增加更多的内容

```js
tspan.text(function(add) {
  add.plain('Just plain text.')
  add.tspan('Fancy text wrapped in a tspan.').fill('#f06')
  add.tspan(function(addMore) {
    addMore.tspan('And you can doo deeper and deeper...')
  })
})

```

`return` : `itself`

### tspan()

在`tspan`的内部嵌入一个`tspan`

```js
tspan.tspan('I am a child of my parent').fill('#f06')
```

`return` : `SVG.Tspan`


### plain()

增加简单的文本

```js
tspan.plain('I do not have any expectations.')
```

`return` : `itself`

### dx()

动态去设置元素的`x`值，跟一个html元素设置了`position:relative`跟`left`是相似的

```js
tspan.dx(30)
```

`return` : `itself`

### dy()

动态去设置元素的`y`值，跟一个html元素设置了`position:relative`跟`top`是相似的

```js
tspan.dy(30)
```

`return` : `itself`


### newLine()

`newLine`方法可以非常方便的利用当前的`leadding（行高）`，去设置`dy`属性，从而创建新的一行

```js
var text = draw.text(function(add) {
  add.tspan('Lorem ipsum dolor sit amet ').newLine()
  add.tspan('consectetur').fill('#f06')
  add.tspan('.')
  add.tspan('Cras sodales imperdiet auctor.').newLine().dx(20)
  add.tspan('Nunc ultrices lectus at erat').newLine()
  add.tspan('dictum pharetra elementum ante').newLine()
})
```

`return` : `itself`

### clear()

清除`tspan`内的所有内容
PS：类似于html元素的`innerHTML = ''`；

```js
tspan.clear()
```

`return` : `itself`

### length()

获得`tspan`的所占的宽度

```js
tspan.length()
```
`return` : `number`


## TexrPath

文本可以沿着路径去运动在SVG中是一个很棒的特性。

```js
var text = draw.text(function(add) {
    add.tspan('We go ')
    add.tspan('up').fill('#f09').dy(-40)
    add.tspan(', then we go down, then up again').dy(40)
})
text
    .path('M 100 200 C 200 100 300 0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
    .font({
        size: 42.5,
        family: 'Verdana'
    })
```

文本元素在调用`path()`方法的时候，就相当于在属性上介于文本跟路径元素之间，从这一点上，文本元素也可以调用`plot()`方法来更新路径。


```js
text.plot('M 300 500 C 200 100 300 0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
```

`<textPath>`特殊的属性可以被应用到`textPath`实例本身

```js
text.textPath().attr('startOffset', 100)
```

当然了，对这些特殊的属性做动画

```js
text.textPath().animate(3000).attr('startOffset', 0.8)
```

`return` : `SVG.TextPath`

继承关系 `SVG.TextPath`  < `SVG.Element`



### textPath()

返回`textPath`的引用

```js
var textPath = text.textPath()
```

`return` : `SVG.TextPath`

### track()

返回textPath所链接的路径

```js
var path = text.track()
```

`return` : `SVG.Path`


## Use

`use`元素可以简单的引用另外一个已经存在的元素。这个已经存在的元素的任何改变都可以直接反应在use的实例上。`use`的用法非常的简单

```js
var rect = draw.rect(100, 100).fill('#f09')
var use  = draw.use(rect).move(200, 200)
```

在例子中的svg出现了2个矩形，一个是原来的，一个是被引用的。在很多情况下，我们想隐藏矩形源，最好的办法是在`defs`节点中创建这个矩形源。

```js
var rect = draw.defs().rect(100, 100).fill('#f09')
var use  = draw.use(rect).move(200, 200)
```

这种方式的话，矩形元素就好像一个储存库那样，你可以去更改它的属性，但是它是不会被渲染出来的。
另一种方法是指向一个外部的svg文件，只需要指明这个元素的id跟路径地址就可以了，就想这样：

```js
var use  = draw.use('elementId', 'path/to/file.svg')
```

这种方式是很有用的，特别是当你已经创建了一个复杂的图形。值得注意的是，外部图形（不同域）可能会需要用XHR的办法去加载。

`return` : `SVG.Use`

继承关系 `SVG.Use`  < `SVG.Shape` < `SVG.Element`

## Symbol

跟`group`元素很像的是，`symbol`元素也是一个容器元素。两者之间的区别是`symbol`它不会被渲染出来。因此`symbol`元素对于用`use`组合来说是完美的。

```js
var symbol = draw.symbol()
symbol.rect(100, 100).fill('#f09')

var use  = draw.use(symbol).move(200, 200)
```

`return` : `SVG.Bare`

继承关系 `SVG.Bare` < `SVG.Element`[浅继承于SVG.Parent]

## Bare

对于SVG.js没有描述的所有SVG元素，`SVG.Bare`这个类就派上用场了。这个类直接从`SVG.Element`继承，并且可以在单独的命名空间里添加自定义方法，这样可以不污染主`SVG.Element`命名空间。给了你很大的发挥空间

PS： 其实就是用来创建svgjs没有说到的标签，例如是一个自定义的标签等等

### element()

`SVG.Bare`可以直接通过父元素`element()`方法实现实例化。

```js
var element = draw.element('title')
```

第一个参数传递的字符串值是创建出来的节点的名字
PS：例如你传个title，就会创建一个title的标签

此外，任何现有的类名都可以作为第二个参数来定义，表明这个新创建元素继承。

```js
var element = draw.element('symbol', SVG.Parent)
```

这给了使用者很大的能力，但是请记住，能力越大，责任越大

`return` : `SVG.Bare`

### words()

`SVG.Bare`实例有一个附带的方法去添加文本

```js
var element = draw.element('title').words('This is a title.')
//-> <title>This is a title.</title>
```

## Referencing elements 获取元素

### by id 通过id

如果你想一个通过id获取`SVG.js`创建出来的元素，那样你可以使用`SVG.get()`

```js
var element = SVG.get('my_element')

element.fill('#f06')
```

### Using Css Selectors 通过样式选择器

有两种通过样式选择器的方式
一种是检索这个SVG的全部，这将检索文档中所有的svg元素并且把它们`SVG.Set`的实例返回。
PS：一定要注意 这个检索的是所有的元素。无论你创建了多少个svg都一样

```js
var elements = SVG.select('rect.my-class').fill('#f06')
```

第二种方法是在父元素里面进行检索

```js
var elements = group.select('rect.my-class').fill('#f06')
```


### Using jQuery or Zepto 使用jQuery或者Zepto

另一种获取元素的方法是使用jQuery和Zepto，这里是一个例子

```js
// add elements
var draw   = SVG('drawing')
var group  = draw.group().addClass('my-group')
var rect   = group.rect(100,100).addClass('my-element')
var circle = group.circle(100).addClass('my-element').move(100, 100)

// get elements in group
var elements = $('#drawing g.my-group .my-element').each(function() {
  this.instance.animate().fill('#f09')
})
```


## Circular reference  重复引用

所有在`SVG.js`里被z实例化之后的元素都是真实存在的节点

### node 节点

```js
element.node
```

`returns`: `node`

### native()

同样可以用`native()`去实现


```js
element.native()
```

`returns`: `node`

### instance

相似的，返回的是节点在`SVG.js`的实例化引用
PS:跟前面的`element.node`、`element.native()`相反，一个是找节点，一个是找节点的实例化
```js
node.instance
```

`returns`: `element`

## 父级引用

每一个元素都有他们父级方法的引用

### parent()

PS:返回父级的引用，或者按照参数返回指定的父级

```js
element.parent();
```

`returns`: `element`

可以选择一个类名或者样式选择器来作为第1个参数传递进去

```js
var draw   = SVG('drawing')
var nested = draw.nested().addClass('test')
var group  = nested.group()
var rect   = group.rect(100, 100)

rect.parent()           //-> returns group
rect.parent(SVG.Doc)    //-> returns draw
rect.parent(SVG.Nested) //-> returns nested
rect.parent(SVG.G)      //-> returns group
rect.parent('.test')    //-> returns nested
```
`returns`: `element`

甚至可以是svg根节点

```js
var draw = SVG('drawing')

draw.parent() //-> returns the wrappig html element with id 'drawing'
//返回的是包裹着svg的id为drawing的节点
```

`returns`: `HTMLNode`

### doc()

检索跟节点svg你可以使用`doc()`

```js
var draw = SVG('drawing')
var rect = draw.rect(100, 100)

rect.doc() //-> returns draw
```

`returns`: `element`

### parents()

通过类型type或者样式选择器获取元素的所有父级(样式选择器可以看`parent()`方法)

```js
var group1 = draw.group().addClass('test')
  , group2 = group1.group()
  , rect   = group2.rect(100,100)

rect.parents()        // returns [group1, group2, draw]
rect.parents('.test') // returns [group1]
rect.parents(SVG.G)   // returns [group1, group2]

```

`returns`: `Array`

## Child reference 子级引用

### first()

获取元素节点的第1个子元素

```js
draw.first()
```

`returns`: `element`

### last()

获取元素节点的最后一个子元素

```js
draw.last()
```

`returns`: `element`

### children()

使用`children`方法可以检索所有的子节点(PS:只有一级，不会检索到子级的子级)，并且返回一个数组

```js
draw.children()
```

`return`: `array`

### each()

`each()`允许你遍历父元素的所有子元素

```js
draw.each(function(index, children) {
  this.fill({ color: '#f06' })
})
```

需要注意的是，`this`在这里指的是当前的child元素

`return`: `itself`

### has()

查看元素是否在于某个父级之中
PS:只能一级，例如:
```js
group > rect
group.has(rect) =>true
nested > group > rect
nested.has(rect) => false
```

```js
var rect  = draw.rect(100, 50)
var group = draw.group()

draw.has(rect)  //-> returns true
group.has(rect) //-> returns false
```

`return`: `boolean`

### index()

返回给定的元素在父元素的位置，从`0`开始。返回`-1`，则表示元素不在该父元素之中。

```js
var rect  = draw.rect(100, 50)
var group = draw.group()

draw.index(rect)  //-> returns 0
group.index(rect) //-> returns -1
```

`return`: `number`

### get(0)

在子级数组中获取指定位置的子级

```js
var rect   = draw.rect(20, 30)
var circle = draw.circle(50)

draw.get(0) //-> returns rect
draw.get(1) //-> returns circle

```

`return`: `element`

### clear()

移除父级中所有的元素

```js
draw.clear()
```

`return`: `itself`

## Import / export SVG 导入导出svg

可以通过`svg()`方法导出完整生成的SVG或一部分

```js
draw.svg()
```

导出所有的元素

导入使用相同的方法

```js
draw.svg('<g><rect width="100" height="50" fill="#f06"></rect></g>')
```

导入适用于用`SVG.Parent`继承的任何元素，基本上每个元素都可以包含其他元素

`getter` `returns`: `string`
`settrt` `returns`: `itself`

## Attributes and styles 属性和样式

### attr()

通过`attr()`，你可以直接的获取或者设置元素的属性
获取单个属性

```js
rect.attr('x')
```

设置单个属性

```js
rect.attr('x', 50)
```

一次性设置多个属性

```js
rect.attr({
    `fill`: '#f06',
    'fill-opacity': 0.5,
    `stroke`: '#000',
    'stroke-width': 10
})

```

使用命名空间设置属性

```
rect.attr('x', 50, 'http://www.w3.org/2000/svg')
```
PS：上面的例子我自己试的时候会让`rect`出现2个`x`的，如果删除了命名空间的话就可以是正常的，可能是这个命名空间只是对于正常有命名空间的标签才有用。
EX：
```
var rect2 = draw.rect(100,100).move(200,200).fill('red');
rect2.attr('x', 50, 'http://www.w3.org/2000/svg');
```

移除属性

```js
rect.attr('fill',null);
```

`getter` `returns`: `value`
`settrt` `returns`: `itself`


### style()

使用`style()`方法，可以像`attr()`管理`attr`那样管理`style`

```js
rect.style('cursor', 'pointer')
```

一次性批量设置样式可以用对象作为参数

```js
rect.style({ cursor: 'pointer', fill: '#f03' })
```

或者是用样式字符串

```js
rect.style('cursor:pointer;fill:#f03;')
```

就像`attr()`一样，`style()`的方法也可以作为`getter`获取样式

```js
rect.style('cursor')
// => pointer
```

甚至是全部的值

```js
rect.style()
// => 'cursor:pointer;fill:#f03;'
```

PS: 这里需要注意的是，如果是一些复合属性例如：`stroke-width` 这种带`-`的，不必按照JS获取样式那样写成驼峰，直接用回原来的即可。

删除单个样式的方法跟`attr()`是一致的。

```js
rect.style('fill',null)
```

`getter` `returns`: `value`
`settrt` `returns`: `itself`

### fill()

`fill()`方法是`attr()`一个很好的替代方法。
PS：只是对于设置`fill`而言

```js
rect.fill({ color: '#f06', opacity: 0.6 })
```

一个十六进制的字符串参数也能够正常的被执行

```js
rect.fill('#f06')
```

最后，但是同样重要的是，你也可以使用图片作填充(fill)，只需要传递图片地址就可以了

```js
rect.fill('images/shade.jpg')
```

PS：SVG会在引用`defs`里面创建一个image，再用这个fill指向这个image

或者你想要更多的控制图片的大小，你也可以传递图片的实例

```js
rect.fill(draw.image('images/shade.jpg', 20, 20))
```
returns`: `itself`

### stroke()

`stroke()`方法跟`fill()`方法很像

```js
rect.stroke({ color: '#f06', opacity: 0.6, width: 5 })
```

跟`fill`一样，也能设置一个十六进制的字符串

```js
rect.stroke('#f06')
```

跟`fill()`方法没有什么不同，你也可以使用图片作为`stroke`，只需要传图片的地址作为参数即可

```js
rect.stroke('images/shade.jpg')
```

或者你想要更多的控制图片的大小，你也可以传递图片的实例

```js
rect.stroke(draw.image('images/shade.jpg', 20, 20))
```

`returns`: `itself`

### opacity()

设置元素的整体不透明度

rect.opacity(0.5)

`returns`: `itself`


### reference()

很多时候，元素都会通过属性链接另外一个元素。使用`reference`方法可以获取这个连接元素的实例，唯一的参数就是这个属性名称

```js
use.reference('href') //-> returns used element instance
// or
rect.reference('fill') //-> returns gradient or pattern instance for example
// or
circle.reference('clip-path') //-> returns clip instance
```

### hide()

隐藏元素
PS:`display:none`

```js
rect.hide()
```

`returns`: `itself`

### show()

显示元素

```js
rect.show()
```

`returns`: `itself`

### visible()

判断元素是否显示

```js
rect.visible()
```

`returns`: `boolean`

## Classes  类名

### classes()

返回包含这个节点所有类名的一个数组

```js
rect.classes()
```

`getter` `returns`: `array`

### hasClass()

判断是否有给定的类

```js
rect.hasClass('purple-rain')
```

`getter` `returns`: `boolean`

### addClass()

增加一个给定的类名

```js
rect.addClass('pink-flower')
```

`setter` `returns`: `itself`

### removeClass()

删除一个给定的类名

```js
rect.removeClass('pink-flower')
```

`setter` `returns`: `itself`


### toggleClass()

对给定的类名进行开关控制

PS：有则删除无则增加class

```js
rect.toggleClass('pink-flower')
```

`setter` `returns`: `itself`

## Size and Position 尺寸和位置

当元素属性是该类型的元素自有属性时，直接设置其属性来定位元素才起作用，但是下面描述的定位方法更加方便，因为它们对于所有元素类型。

举个例子，下面的代码都能够正常运行，因为每一个元素都能通过本身的属性进行定位.

```js
rect.attr({ x: 20, y: 60 })
circle.attr({ cx: 50, cy: 40 })
```

矩形的距离是按照矩形的左上角来移动的，圆则是按照中心点。然而试图让圆按照左上角移动或者矩形按照中心去移动都是无效的。下面的代码将会是无效的，因为他们所设置的属性并不是他们本身所拥有的。

```js
rect.attr({ cx: 20, cy: 60 })
circle.attr({ x: 50, y: 40 })
```

然而，下面描述定位的方法将适用于所有的元素，而不管该属性是不是该元素的固有属性。所以它不像上面的代码那样，这些代码都可以被正常的执行.

```js
rect.cx(20).cy(60)
circle.x(50).y(40)
```
PS: 这里的话rect会按照中心点进行偏移左上角(20, 60),circle会按照左上角偏移(50, 40)


然而，重要的是要注意，这些方法仅仅适用于无单位的用户坐标系。例如，如果元素的大小是通过百分比或其他单位设置，那么通过自有属性去定位方法很有可能依然有效，但是解决非自有属性的定位方法将会产生其他无法预计的结果，比如`getter`和`setter`

```js
rect.cx('20px').cy('60px') //出来的是0，0
circle.x('50px').y('40px') // 出来0,0
```

### size()

用`width`和`height`设置元素的尺寸

```js
rect.size(200, 300)
```

省略高度也可以按比例调整大小
PS:对于svg本身来说，一个参数只会为它设置宽度，高度还是0的，所以应该是只对部分节点有效果，例如image

```js
rect.size(200)
```

或者也可以设置`width`为null

```js
rect.size(null, 200)
```

与定位不一样，元素的尺寸可以通过`attr()`设置。因为当每种类型的元素改变他们的各种尺寸，用`size()`方法都是非常合适的。

有一个是例外的，对于`SVG.Text`元素，方法只需要传1个参数，并将它的值给`font-size`

`returns`: `itself`

### width()

设置元素的`width`

```js
rect.width(200)
```

这个方法也可以获取元素的`width`

```js
rect.width() //-> returns 200
```

`getter` `returns`: `value`
`setter` `returns`: `itself`

### height()

设置元素`width()`

```js
rect.height(325)
```

也可以获取元素的`height`

```js
rect.height() //-> returns 325
```

`getter` `returns`: `value`
`setter` `returns`: `itself`


### radius()

`circle`、`ellipses`和`rect`都会需要用到`radius()`方法，对于`rect`，它定义的是圆角。对于`circle`，它定义的是`r`属性，也就是半径属性

```js
circle.radius(10)
```

对于`ellipses`跟`rect`，传递两个参数以单独设置`rx`和`ry`属性，或者设置一个属性让两个值相等

```js
ellipse.radius(10, 20)
rect.radius(5)
```

此功能需要包含在默认分发中的sugar.js模块。

`returns`: `itself`

## move()

根据左上角移动元素到指定的`x`,`y`位置

```js
rect.move(200, 350)
```

`returns`: `itself`

### x()

根据左上角位置，在x方向上移动元素

```js
rect.x(200)
```

无参数的时候`x()`方法可以获取出x坐标值

```js
rect.x() //-> returns 200
```

`getter` `returns`: `value`
`setter` `returns`: `itself`

### y()

根据左上角位置，在y方向上移动元素

```js
rect.y(350)
```

无参数的时候`Y()`方法可以获取出y坐标值

```js
rect.y() //-> returns 350
```

`getter` `returns`: `value`
`setter` `returns`: `itself`

### center()

根源元素中心位置位置，移动元素

```js
rect.center(150, 150)
```

`returns`: `itself`


### cx()

根据元素中心位置，在x方向上移动元素

```js
rect.cx(200)
```

无参数的时候，可以获取元素中心在x方向的位置值

```js
rect.cx() //-> returns 200
```

`getter` `returns`: `value`
`setter` `returns`: `itself`

### cy()

根据元素中心位置，在y方向上移动元素

```js
rect.cy(350)
```

无参数的时候，可以获取元素中心在y方向的位置值

```js
rect.cy() //-> returns 350
```

`getter` `returns`: `value`
`setter` `returns`: `itself`

### dmove()

根据元素现有的位置，在x，y方向上移动元素
PS:不是通过设置translate来算的，而是根据元素的位置 然后根据传参值去动态计算相对位置
```js
rect.dmove(10, 30)
```

`returns`: `itself`

### dx()

根据元素现有的位置，在x方向上移动元素

```js
rect.dx(200)
```

`returns`: `itself`

### dy()

根据元素现有的位置，在y方向上移动元素

```js
rect.dy(200)
```

`returns`: `itself`

## Document tree manipulations 文档树操作

### clone()

要准确的复制一个元素，用`clone()`就很方便

```js
var clone = rect.clone()
```

`returns`: `element`

这将创建一个新的，没有链接的复制副本。如果要创建带链接的克隆，请参阅use元素。默认情况下，克隆元素会防止在原始元素之后。当parent参数传给clone()方法时，它会将克隆的元素附加到给定的parent.

PS: 这里说的链接应该是那种类似fill(#id)的这种类似引用吧。

```js
var group = draw.group();
var clone = rect.clone(group)
```

### remove()

在svg节点中移除给定的元素

```js
rect.remove()
```

`returns`: `itself`

### replace()


在svg文档中的找到元素的位置，通过这个方法用参数元素替换掉找到的元素
PS:下面的例子是用圆替换矩形,返回的element是参数的这个element而不是调用的element

```js
rect.replace(draw.circle(100))
```

`returns`: `element`

### add()

将调用的参数设置为父级的子节点，并返回父节点

```js
var rect = draw.rect(100, 100)
var group = draw.group()

group.add(rect) //-> returns group
```

`returns`: `itself`

### put()

将调用的参数设置为父级的子节点，并返回子节点

```js
group.put(rect) //-> returns rect
```

`returns`: `element`

### addTo()

将调用元素作为一个子节点，并返回子节点

```js
rect.addTo(group) //-> returns rect
```

`returns`: `itself`

### putIn()

将调用元素作为一个子节点，并返回父节点
```js
rect.putIn(group) //-> returns group
```

`returns`: `element`


### toParent()

将元素移动到不同的父级(类似于`addTo()`)，但不更改其可是变化。所有的变换都合并并应用于元素

```js
rect.toParent(group) // looks the same as before
```

`returns`: `itself`

### toDoc()

跟`toParent()`是一样的，不过是增加到根目录

`returns`: `itself`

### ungroup() / flatten()

分解`group`或者容器，并将所有的元素移动到给定的父节点，而不更改其可视化显示。带来的结果是svg的结构是扁平的。就像例子这样：

```js
// ungroups all elements in this group recursively and places them into the given parent
// 取消此分组中的所有元素，并将它们放置到给定的父级
// (default: parent container of the calling element)
// 默认放置到父容器
group.ungroup(parent, depth)

// call it on the whole document to get a flat svg structure
// 整个文档中调用它会是的整个svg都是一个扁平的结构
drawing.ungroup()

// breaks up the group and places all elements in drawing
// 分解组并将所有元素放置到drawing中
group.ungroup(drawing)

// breaks up all groups until it reaches a depth of 3
// 打破所有的分组，从第几级开始
drawing.ungroup(null, 3)

// flat and export svg
var svgString = drawing.ungroup().svg()
```

自己的试验

```js
var group = draw.group().group().group().group().group();
group.add(draw.rect(100,50100));
group(draw,2); // 第2级开始打破，因为都是分组，所以什么情况都没发生 只有当 depth 为5的时候才会出现完全打破

var group2 = draw.group();
group2.add(draw.rect(50, 50))
group2(draw,1); //解除所有分组，rect在draw上
```

PS： 第2个参数应该是用来指定如果一个元素的层级超过了n层，当且仅当需要解绑的第n层有不属于group的元素才能解绑，否则不解绑。个人感觉是如果需要解绑全部那样只需要写1个参数就可以了。第2个参数的作用就是为了解绑如果一个group中同时含有group跟非group的时候才有用。

`returns`: `itself`

## Transforms 变换

### transform()

`transform()`无参数的时候可以被当做一个`getter`

```js
element.transform()
```

返回值是一个`object`,包含下面的这些值：

* x
* y
* skewX
* skewY
* rotation
* cx
* cy

PS：上面是官方写得，但是我自己写的时候还包括了下面这几个

* transformedX
* transformedY
* matrix:{a,b,c,d,e,f}
* a
* b
* c
* d
* e
* f

PS： abcdef应该是那个矩阵变化的值


此外还可以传递一个字符串用于获取

```js
element.transform('rotation')
```

在这个例子中返回回来的是一个`number`值

当作`setter`的时候，它有两种方式去设置。默认情况下变换是绝对的，就像下面这样，如果你调用
PS：大概的意思是，他只会取最后的那个值

```js
element.transform({ rotation: 125 }).transform({ rotation: 37.5 })
```

这个旋转的结果是`37.5`，而不是两个变换相加。但是如果这是你想要的话，那么可以添加一个`relative`参数。就像这样

```js
element.transform({ rotation: 125 }).transform({ rotation: 37.5, relative: true })
```

或者，可以将`relative`作为第2个参数传递

```js
element.transform({ rotation: 125 }).transform({ rotation: 37.5 }, true)
```

可用的转换还包括

* rotation 可选`cx`或`cy`
* scale 可选`cx`或`cy`
* scaleX 可选`cx` 或cy
* scaleY
* skewX
* skewY
* x
* y
* a,b,c,d,e,f 或者一个矩阵变换对象`matrix`

PS：他这里的意思是，你这个object对象里面如果是左边的那些，那么还可以选上那些可选的，可选之外的就不行了
```js
rect2.animate('2s').transform({ rotation: 90 ,cx:90,cy:100});
```
`getter` `returns`: `value`

`setter` `returns`: `itself`

### rotate()

`rotate()`方法，将元素自身为中心旋转元素

```js
// rotate(degrees)
rect.rotate(45)
```

也可以自定义一个旋转点

```js
// rotate(degrees, cx, cy)
rect.rotate(45, 50, 50)
```

`returns`: `itself`

### skew()

`skew()`方法需要一个`x`和`y`值

```js
// skew(x, y)
rect.skew(0, 45)
```

`returns`: `itself`

### scale()

`scale()`方法需要一个`x`和`y`值

```js
// scale(x, y)
rect.scale(0.5, -1)
```

`returns`: `itself`

### translate()

`translate()`方法需要一个`x`和`y`值

```js
// translate(x, y)
rect.translate(0.5, -1)
```
`returns`: `itself`

## Geometry 盒子模型

### viewbox()

可以使用`viewbox()`方法管理`<svg>`的viewBox属性。当提供四个参数时，它将作为一个`setter`

```js
draw.viewbox(0, 0, 297, 210)
```

或者你可以提供一个`object`作为第一个参数

```js
draw.viewbox({ x: 0, y: 0, width: 297, height: 210 })
```

当没有任何参数的时候，会返回一个`SVG.ViewBox`的实例

```js
var box = draw.viewbox()
```

但是`viewbox()`方法的最好的事情是你可以获得viewbox的缩放

```js
var box = draw.viewbox()
var zoom = box.zoom
```

如果svg的viewbox尺寸跟svg在画布上的尺寸是相同的话，那么这个zomm将会是1

`getter` `returns`: `SVG.ViewBox`

`setter` `returns`: `itself`

### bbox();

获取元素的边界，用的是原生的`getBBox()`，只是在外面包了一层并增加了更多的值

```js
path.bbox()
```

这会返回一个`SVG.BBox`的实例，包括下面的这些值

* width (来自原生的`getBBox`)
* height (来自原生的`getBBox`)
* w (简写 `width`)
* h (简写 `height`)
* x (来自原生的`getBBox`)
* y (来自原生的`getBBox`)
* cx (中心点距离边界x方向的距离)
* cy (中心点距离边界y方向的距离)
* x2 (边界右下角的x距离)
* y2 (边界右下角的y距离)

`SVG.BBox`还有一个很好的小功能，`merge()`方法。使用`merge()`可以将两个`SVG.BBox`实例合并成一个新的实例，这样新的边界就会是两个边界合并到一起了。

PS：原来的时候我们做bbox的时候只能获取单个元素的，merge2个元素的话，那么2个元素就会组成一个新的边界模型。


```js
var box1 = draw.rect(100,100).move(50,50)
var box2 = draw.rect(100,100).move(200,200)

var box3 = box1.merge(box2)
```

`returns`: `SVG.BBox`

### tbox()

`bbox()`返回的边框值中是不考虑任何变换的，`tbox()`方法会考虑，因此，任何平移或缩放都将应用于结果值以更接近的视觉表示


PS: 对于`bbox`，下面这两个调用都是一样的

```js
var rect = draw.rect(100,100).move(50,50)
var box1 = rect.bbox()
```

transform

```js
var rect = draw.rect(100,100).move(50,50)
var box1 = rect.bbox()
rect.transform({x:100,y:100})
```

对于使用`tbox`则会不一样
PS:只有部分值会受到影响，并不是全部

```js
path.tbox()
```

这会返回一个`SVG.TBox`的实例，包含下列这些值

* width (来自原生的`getBBox`，但是会受到矩阵中`scaleX`的影响)
* height (来自原生的`getBBox`，但是会受到矩阵中`scaleY`的影响)
* w (简写 `width`)
* h (简写 `height`)
* x (来自原生的`getBBox`，但是会受到矩阵中`x`的影响)
* y (来自原生的`getBBox`，但是会受到矩阵中`y`的影响)
* cx (中心点距离边界x方向的距离)
* cy (中心点距离边界y方向的距离)
* x2 (边界右下角的x距离)
* y2 (边界右下角的y距离)

请注意，元素的旋转并不会添加到计算当中

`returns`: `SVG.TBox`

### rbox()

跟`bbox()`有点像，但是会考虑所有的变换，告诉你元素的确切位置。

```js
path.rbox()
```

这个会返回`SVG.RBox`的实例，包含下面这些值

* width (真实的宽度)
* height (真实的高度)
* w (简写 `width`)
* h (简写 `height`)
* x (在x方向上的真实位置)
* y (在y方向上的真是位置)
* cx (中心在x方向上的真实位置)
* cy (中心在y方向上的真实位置)
* x2 (右下在x方向上的真实位置)
* y2 (右下在y方向上的真实位置)

重要：在Mozilla浏览器或者其他浏览器之中，可能会不能正常的识别出`stroke`的宽度。因为，在Mozilla浏览器或者其他浏览器生成的盒模型可能会不同。这是很难改变的，所以目前这算是一个不便，我们也没辙。

`returns`: `SVG.RBox`

### ctm()

相对于最近的父级的视图返回元素当前的变换矩阵

```js
path.ctm()
```

`returns`: `SVG.Matrix`































































2
