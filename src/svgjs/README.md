# SVG.js

网上好像没有相关的中文文档，这样的话我就自己写一个了，按照官方那边的来写

## 用法
### Create an SVG document(创建一个SVG的节点)
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

### 检查是否支持svg(Checking for SVG support)
在使用SVG.js的时候，它是默认你支持svg的，但是你可以使用它提供的方法去检测是否支持svg

```javascript
if (SVG.supported) {
  var draw = SVG('drawing')
  var rect = draw.rect(100, 100)
} else {
  alert('SVG not supported')
}
```

### SVG document(SVG节点)
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

### Sub-pixel offset fix(子元素的偏移)
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

### Main svg document(SVG根节点)

SVG.js主要的初始化方法是在一个给定的节点中创建一个svg的根节点标签，并且返回一个`SVG.Doc`实例

```javascript
var draw = SVG('drawing')
```
`return`: `SVG.Doc`

继承关系 `SVG.Doc` < `SVG.Container` < `SVG.Parent`

### Nested SVG(嵌套svg)

利用这个方法，你可以嵌套在任意一个节点之中。这个嵌套的的svg所拥有的特性方法跟顶层svg节点是一样的。

`return`: `SVG.Nested`

继承关系 `SVG.Nested` < `SVG.Container` < `SVG.Parent`

### Groups(分组)

如果你想transform一组元素，那么对元素分组是非常有用的。所有的节点都在一个分组里面，这样能很好的保证他们所属的相对位置，一个`group`能像svg根节点那样，拥有所有的调用节点方法

```javascript
var group = draw.group()
group.path('M10,20L30,40')
```

如果一个节点已经存在的haunted，也可以把它增加进去一个`group`里面
`group.add(rect)`

注意： `groups`是没有大小的，它依赖于它的内容。因此它是没有`x`,`y`,`width`跟`height`,如果你需要这些的话，使用`nested()`svg去代替`group`

`return`: `SVG.G`

继承关系 `SVG.G` < `SVG.Container` < `SVG.Parent`


### Hyperlink(超链接)
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


### Defs(引用)

`<defs>`节点是一个放置引用元素的容器。它的所有后代元素都不会被渲染。`<defs>`会一直存在于svg节点只用，并且可以利用`defs()`来进行存取。

```
var defs = draw.defs()
```

defs还可以通过另一个元素的`doc()`方法获取到

```javascript
var defs = rect.doc().defs()
```

`return`: `SVG.A`

继承关系 `SVG.A` < `SVG.Container` < `SVG.Parent`

## Rect(矩形)

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
``
`
这样的会设置他们的`rx`和`ry`都是10，如果要分别设置的话，可以向下面这样

```javascript
rect.radius(10, 20)
```

`returns`: `itself`

## Circle(圆)

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

## Ellipse(椭圆)

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


## Line(直线)

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

### 坐标点(array())

参考于 `SVG.PointArray`的实例，这个方法只能限于内部使用  
方法的作用是一个数组，数组的每一个值都是包含x,y的点
`returns`: `SVG.PointArray`

## Polyline(折线)

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

## Polygon(多边形)

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


## Path(路径)

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

## Image(图片)

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

### leading() (间距的意思)

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

```js

text.clear()

```

`return` : `itself`

### length()

获得计算之后文本的长度

PS：这里获取出来的是文字所占的内容宽度，而不是文字的长度，两者是有区别的，
其内部使用的是`getComputedTextLength`来获取的。

`return` : `itself`

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

### dy()

动态去设置元素的`y`值，跟一个html元素设置了`position:relative`跟`top`是相似的

```js

tspan.dy(30)

```








2
