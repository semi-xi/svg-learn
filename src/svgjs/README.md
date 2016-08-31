# SVG.js

网上好像没有相关的中文文档，这样的话我就自己写一个了，按照官方那边的来写

## 用法
### 创建一个SVG的节点(Create an SVG document)
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

### SVG节点(SVG document)
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

### 子元素的偏移(Sub-pixel offset fix)
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

### SVG根节点(Main svg document)

SVG.js主要的初始化方法是在一个给定的节点中创建一个svg的根节点标签，并且返回一个`SVG.Doc`实例

```javascript
var draw = SVG('drawing')
```
`return`: `SVG.Doc`

继承关系 `SVG.Doc` < `SVG.Container` < `SVG.Parent`

### 嵌套svg(Nested SVG)

利用这个方法，你可以嵌套在任意一个节点之中。这个嵌套的的svg所拥有的特性方法跟顶层svg节点是一样的。

`return`: `SVG.Nested`

继承关系 `SVG.Nested` < `SVG.Container` < `SVG.Parent`

### 分组(Groups)

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


### 超链接(Hyperlink)
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


### 引用(Defs)

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

## 矩形(Rect)

Rect 有2个参数，他们分别是`width`和`height`

```javascript
var rect = draw.rect(100, 100)
```

`return`: `SVG.Rect`

继承关系 `SVG.Rect` < `SVG.Shape` < `SVG.Element`


### 半径弧度(radius())

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

## 圆(circle)

它只有一个参数，这个参数是它的直径

```javascript
var circle = draw.circle(100)
```

`return`: `SVG.Circle`

继承关系 `SVG.Circle` < `SVG.Shape` < `SVG.Element`

### 半径(radius())

重新定义它的半径

```javascript
circle.radius(75)
```

`returns`: `itself`

## 椭圆(Ellipse)

跟矩形一样，有两个参数`width`,`height`

```javascript
var ellipse = draw.ellipse(200, 100)
```

`return`: `SVG.Ellipse`

继承关系 `SVG.Ellipse` < `SVG.Shape` < `SVG.Element`

### 半径(radius())

允许重新设置两个半径

```javascript
ellipse.radius(75, 50)
```

`returns`: `itself`


## 直线(Line)

创建一条从点a到点b的直线

```javascript
var line = draw.line(0, 0, 100, 150).stroke({ width: 1 })
```

生成了的直线改变它有4种方式，可以看下`plot()`方法查阅所有的用法

### plot()

```javascript
```
