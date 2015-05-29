Readme 
===

# 创建Raphael
* Raphael(x,y,w,h)  ||  Raphael(ele,w,h,cb ）|| Raphael(all,vb)
创建一个svg，这个svg是绝对定位的，距离左边x距离 距离顶部y距离，宽高为w和h  
ele是一个id名字或者是一个id节点
all 是一个图形数组数组与Paper.add类似
```javascript
// Each of the following examples create a canvas
// that is 320px wide by 200px high.
// Canvas is created at the viewport’s 10,50 coordinate.
var paper = Raphael(10, 50, 320, 200);
// Canvas is created at the top left corner of the #notepad element
// (or its top right corner in dir="rtl" elements)
var paper = Raphael(document.getElementById("notepad"), 320, 200);
// Same as above
var paper = Raphael("notepad", 320, 200);
// Image dump
var set = Raphael(["notepad", 320, 200, {
    type: "rect",
    x: 10,
    y: 10,
    width: 25,
    height: 25,
    stroke: "#f00"
}, {
    type: "text",
    x: 30,
    y: 40,
    text: "Dump"
}]);
```

*  Raphael.angle(x1,y1,x2,y2[x3,y3]) 返回两点之间的角度 
```javascript
Raphael.angle(10,50,100,120)
```

* Raphael.animation(params, ms, [easing], [callback])  
params 要变化的属性 ms 持续时间 easing 运动曲线 cb 回调函数


* Raphael.bezierBBox(…) 三次贝塞尔曲线 里面是6个值 或者是一个数组

* Raphael.color(rsl)  返回一个颜色值的解析 支持rgb,hsl ,#xxx,#xxxxxx

* Raphael.deg(deg) 返回transform的角度

* Raphael.easing_formulas 运动曲线 
  *  linear 直线
  *  < 或者easeIn 或者ease-in 先减速再加速
  *  \> 或者easeOut 或者ease-out 先加速再减速
 *  < > 或者easeInOut 或者 ease-in-out
 *  backIn 或者 back-in  一开始就弹性
 *  backOut 或者back-out 最后弹性
 * bounce 慢速弹性运动
*  elastic 超快速弹性运动



* Raphael.el 自定义函数   ！！是给这个节点或者是图形加的 跟之后说的 Raphael.fn差别在这里
``` javascript
Raphael.el.red = function () {
    this.attr({fill: "#f00"});
};
// then use it
paper.circle(100, 100, 20).red();
    
```

* Raphael.findDotsAtSegment(p1x, p1y, c1x, c1y, c2x, c2y, p2x, p2y, t) 找到点坐标对应的三次贝塞尔曲线

* Raphael.fn  自定义函数 ，函数是绑在Raphael上的
``` javascript
Raphael.fn.arrow = function (x1, y1, x2, y2, size) {
    return this.path( ... );
};
// or create namespace
Raphael.fn.mystuff = {
    arrow: function () {…},
    star: function () {…},
    // etc…
};
var paper = Raphael(10, 10, 630, 480);
// then use it
paper.arrow(10, 10, 30, 30, 5).attr({fill: "#f00"});
paper.mystuff.arrow();
paper.mystuff.star();
```

* Raphael.format (taken,...) 替换
``` javascipt
var x = 10,
    y = 20,
    width = 40,
    height = 50;
// this will draw a rectangular shape equivalent to "M10,20h40v50h-40z"
paper.path(Raphael.format("M{0},{1}h{2}v{3}h{4}z", x, y, width, height, -width));
```

* Raphael.fullfill(token, json)  更加先进的一直替换
``` javascript
// this will draw a rectangular shape equivalent to "M10,20h40v50h-40z"
paper.path(Raphael.fullfill("M{x},{y}h{dim.width}v{dim.height}h{dim['negative width']}z", {
    x: 10,
    y: 20,
    dim: {
        width: 40,
        height: 50,
        "negative width": -40
    }
}));
```

* Raphael.getColor([value])  获取光谱上的下一个颜色值  value 默认是0.75 


* Raphael.getColor.reset() 重置光谱颜色最初为红色

* Raphael.getPointAtLength(path, length) 获取在路径上长度为length的坐标 返回是一个json { x: ,y: ,alpha: } 最后一个是角度

* Raphael.getRGB（colour） 解析这个颜色值 返回一个json形式的颜色分析  colour的格式可以是'red','#xxx''#xxxxxx''rgb()' 'hsl'  
    返回的格式
    ```
    {
    r  red
    g green
    b blue
    hex   #xxx
    error 
}
    ```

* Raphael.getSubpath(path,form,to) 截取一段路径   返回的是一个字符串路径
 path 路径( string) from 开头  to 结束 

* Raphael.getTotalLength(path) 返回这个路径的长度 返回的是长度px

* Raphael.hsl(h,s,l) hsl转换成16进制的颜色值 类似Raphael.hsb(h,s,l)

* Raphael.hsl2rgb(h, s, l)  hsl转换rgb 类似还有 Raphael.hsb2rgb(h, s, b)

* Raphael.isBBoxIntersect(bbox1,bbox2) 判断两个元素的boundingbox是否相交

* Raphael.isPointInsideBBox(bbox , x , y) 判断这个点是否在盒子里面

* Raphael.isPointInsidePath(path,x,y) 判断点是否在这条路径上

* Raphael.mapPath(path, matrix) 对路径进行矩阵变换

* Raphael.matrix(a, b, c, d, e, f)  返回一个线性变换

* Raphael.parsePathString(pathString) 解析这个路径 返回一个包含路径字符串的数组

* Raphael.parseTransformString(TString) 解析这个transform 传回一个数组

* Raphael.pathBBox(path) 给定路径返回一个盒子模型 返回是一个json {x,y,x2,y2,width,height}

* Raphael.pathIntersection(path1,path2) 找到两个路径的一个相交点  
```  
return 
[
{
x:[number] x coordinate of the point
y:[number] y coordinate of the point
t1:[number] t value for segment of path1 //第1个线段
t2:[number] t value for segment of path2 //第2个线段
segment1:[number] order number for segment of path1
segment2:[number] order number for segment of path2
bez1:[array] eight coordinates representing beziér curve for the segment of path1
bez2:[ array]eight coordinates representing beziér curve for the segment of path2
}
]
```

* Raphael.rad(deg)  角度转换弧度 感觉是超级有用的东西

*  Raphael.registerFont(font) 定义一个新的字体

*  Raphael.rgb2hsl(r, g, b) rgb 转hsl

* Raphael.setWindow(newwin) 设置一个新的窗口 类似iframe

* Raphael.st 给节点增加方法或者设置值  
与Raphael.el的不同
```javascript
Raphael.el.red = function () {
    this.attr({fill: "#f00"});
};
Raphael.st.red = function () {
    this.forEach(function (el) {
        el.red();
    });
};
// then use it
paper.set(paper.circle(100, 100, 20), paper.circle(110, 100, 20)).red();
```

* Raphael.svg 监测浏览器是否支持svg

* Raphael.toMatrix(path, transform) 返回变换到路径的一个矩阵 

* Raphael.transformPath(path, transform)  返回路径经过变化后的路径

* Raphael.type 判断浏览器是支持svg还是vml (ie独有)

* Raphael.vml 判断浏览器是否支持vml
  
  

# Paper(这里指的是svg)
* Paper.add(arg)增加内容到svg上    
arg是一个数组的形式，每一个值都是一个json，type确定一个创建的内容

* Paper.bottom  
找到在svg在表现上为最下面的一个元素

* Paper.ca  Paper.customAttributes 的简写 自定义属性  
```javascript
paper.customAttributes.hue = function (num) {  
    num = num % 1;  
    return {fill: "hsb(" + num + ", 0.75, 1)"};  
};  
var c = paper.circle(10, 10, 10).attr({hue: .45});  
// or even like this:    
c.animate({hue: 1}, 1e3);  
```

* Paper.circle(x, y, r)  画一个圆;

* Paper.clear() 清除svg的所有内容

* Paper.ellipse(x, y, rx, ry) 椭圆 增加一个椭圆 x，y坐标 rx，ry半径;

* Paper.forEach(cb,thisArg) 跟jq一样 循环这个paper的节点 详细见<code>foreach.html</code>

* Paper.getById(id) 获得某个id节点  //表示我用attr加不上去 目前不知道怎么加id

* Paper.getElementByPoint(x, y)   返回给定点的最上面的一个元素
``` javascript
paper.getElementByPoint(mouseX, mouseY).attr({stroke: "#f00"})
````

* Paper.getFont(family,[weight],[style],[stretch]) 暂时不明   
 font-family font-weight font-style font-stretch 拉伸  
 ``` javascript
paper.print(100, 100, "Test string", paper.getFont("Times", 800), 30);
````

* Paper.image(src,x,y,w,h)   增加一个图像  
 地址 起始坐标 宽高
 
 * Paper.path(pathString) 路径 按照基本原则 M、Z、L、H、V、C、S、Q、T、A、R
  ``` javascript
 var c = paper.path("M10 10L90 90");
 ````
 
 * Paper.print(x, y, string, font, [size], [origin], [letter_spacing])  
 在某个位置写上文字 x，y位置  string 文字  font 见getFont size文字大小 默认16 origin 对齐方式 baseline  middle(默认) 间隔 默认0
   ``` javascript
 print(10, 50, "print", r.getFont("Museo"), 30).attr({fill: "#fff"});
  ````
  
  * Paper.raphael 这个obj/function 具体不明
  
  * Paper.rect(x,y,w,h,[r])  
创建矩形  最后的参数是弧度 默认0

* Paper.remove()  
移出某个元素

* Paper.renderfix()  
在firefox 和ie9中，当这个图形有依赖其他节点的时候，他的位置会发生变化，使这个图形固定 

* Paper.safari() 解决Safari 强制渲染的问题
  
* Paper.set()  分组，对分组内的元素进行统一操作但是不会创建新的元素  详细见<code>set.html</code> 
  ``` javascript
  var st = paper.set();
st.push(
    paper.circle(10, 10, 5),
    paper.circle(30, 10, 5)
);
st.attr({fill: "red"}); // changes the fill of both circles
```

* Paper.setStart() 结合 Paper.setFiniash()使用 创建Paper.set 所有的东西都会被创建进这个set里面，直到遇到Paper.setFinish

* Paper.setFinish()
``` javascript
paper.setStart();
paper.circle(10, 10, 5),
paper.circle(30, 10, 5)
var st = paper.setFinish();
st.attr({fill: "red"}); // changes the fill of both circles
```

* Paper.setViewBox(x,y,w,h,fit)  
设置视窗  最后一个参数表示是有需要用图形去填充边框

* Paper.text(x,y,text) 设置文字 需要换行就加个\n
``` javascript
    var t = paper.text(50, 50, "Raphaël\nkicks\nbutt!");
```

* Paper.top 找到表现上最上面的那个元素 与Paper.bottom相反  

# Element 对节点的操作
* Element.animate(…)  
```javascript
    {
    params 参数列表
    ms 时间
    easing 运动曲线
    callback
    } or
    animtion [Raphael.animation]
```
* Element.animateWith(…) 动画给另外一个元素
* Element.attr(…) 给节点增加一个属性
```
(attrName,value)
(params)
(arrtName)
(attrNames)
```

* Element.click(handler) 给每一个节点增加点击方法

* Element.clone() 克隆这些节点

* Element.data 增加或者检索整个节点的key（暂时不知道是什么鬼）
```
for (var i = 0, i < 5, i++) {
    paper.circle(10 + 15 * i, 10, 10)
         .attr({fill: "#000"})
         .data("i", i)
         .click(function () {
            alert(this.data("i"));
         });
}
```

* Element.dblclick(handler) 双击

* Element.drag(onmove, onstart, onend, [mcontext], [scontext], [econtext]) 节点的拖拽事件

* Element.getBBox(isWithoutTransform)  返回一个没有经过transform的合模型

* Element.getPointAtLength(length)   返回的坐标点位于给定长度在给定的路径。仅适用于元素的“路径”
```
{
x:r x coordinate
y: y coordinate
alpha: angle of derivative
}
```
* Element.getSubpath(from, to) 返回从开始点到结束点的一个路径

* Element.getTotalLength() 返回路径长度

* Element.glow([glow]) 给元素周围增加辉光（跟阴影差不多）
```
{
width   number	size of the glow, default is 10
fill    boolean	will it be filled, default is false
opacity number	opacity, default is 0.5
offsetx number	horizontal offset, default is 0
offsety number	vertical offset, default is 0
color   string	glow colour, default is black
}
```
* Element.hide()  隐藏元素

* Element.hover(f_in, f_out, [icontext], [ocontext]) 移入移出
```
{
f_in  fn
f_out fn
icontext  context for hover in handle
ocontext context for hover in handle
}
```
* Element.id 设置id

* Element.insertAfter() 在当前的节点后插入一个object
* Element.insertBefore() 在当前的节点前插入一个object

* Element.isPointInside(x, y) 判断这个点是否在图形里面

* Element.mousedown(handler)  mosedown事件

* Element.mousemove(handler) mousemove事件

* Element.mouseout(handler)   mouseout事件

*  Element.mouseover(handler) mouseover事件

* Element.mouseup(handler)  mouseup事件

* Element.onDragOver(f) dragover事件

*  Element.pause([anim]) 暂停动画

* Element.paper 用于元素的扩展
```
Raphael.el.cross = function () {
    this.attr({fill: "red"});
    this.paper.path("M10,10L50,50M50,10L10,50")
        .attr({stroke: "red"});
}
```

* Element.remove() 移出这个节点

* Element.removeData([key]) remove这个key跟之前设置data相反

* Element.resume([anim]) 继续播放，目测是

* Element.rotate(deg, [cx], [cy]) 旋转，后两个参数是旋转中心点

* Element.setTime(anim, value) 设置动画时间

* Element.show() 相对 hide

* Element.status([anim], [value])  获取 或者设置这个元素的动画状态

* Element.scale(sx, sy, [cx], [cy]) 缩放，后两个是缩放中心点

* Element.stop([anim]) 停止动画

* Element.toBack() 移动这个节点到其他节点的后面，以至于让浏览者看到的是最后面的

* Element.toFront()  移动这个节点到其他节点的前面，以至于让浏览者看到的是最上面的

* Element.touchcancel(handler) 触摸取消事件（移动端也能支持？）

* Element.touchend(handler) touchend 事件

* Element.touchmove(handler) touchmove 事件

*  Element.touchstart(handler) touchstart事件

* Element.transform([tstr]) transform变换
```
t translate t100,0  横向移动100 垂直0
r rotate r30,0,100 顺时针30度 旋转点为0,100
s scale s1.5,2,100 放大1.5 放点中心点2,100
//
var el = paper.rect(10, 20, 300, 200);
// translate 100, 100, rotate 45°, translate -100, 0
el.transform("t100,100r45t-100,0");
// if you want you can append or prepend transformations
el.transform("...t50,50");
el.transform("s2...");
// or even wrap
el.transform("t50,50...t-50-50");
// to reset transformation call method with empty string
el.transform("");
// to get current value call it without parameters
console.log(el.transform());
```
* Element.translate(dx, dy) translate 变换

*  Element.unclick(handler) 移出click事件

* Element.undblclick(handler) 移除双击事件

* Element.undrag() 移除drug

* Element.unhover(f_in, f_out) 移除hover时间 包括f_in,f_out

* Element.unmousedown(handler) 移除mousedown事件

* Element.unmousemove(handler)  移除mousemove事件

* Element.unmouseout(handler) 移除mouseout事件

* Element.unmouseover(handler) 移除mouseover事件

* Element.unmouseup(handler) 移除mouseoverup事件

* Element.untouchcancel(handler) 移除 touchcancel事件
 
* Element.untouchend(handler) 移除touchend事件

* Element.untouchmove(handler) 移除touchmove事件

* Element.untouchstart(handler) 移除touchstart事件



#Matrix 矩阵
*  Matrix.add(a, b, c, d, e, f, matrix)  增加一个新的矩阵

* Matrix.clone() 克隆一个新的矩阵

* Matrix.invert() 返回矩阵的相反矩阵 

*  Matrix.rotate(a, x, y) 旋转矩阵

*Matrix.scale(x, [y], [cx], [cy]) 缩放矩阵

*Matrix.split()  反编译矩阵
```
{
dx	number translation by x
dy	number translation by y
scalex	number	scale by x
scaley	number	scale by y
shear	number	shear 剪切
rotate	number	rotation in deg
isSimple	boolean	could it be represented via simple transformations 是否可以通过简单转换
}
```

* Matrix.toTransformString() 矩阵转换transform 字符串形式

* Matrix.translate(x, y) translate 转换成矩阵

* Matrix.x(x, y)  返回给定的X坐标点变换后的矩阵描述 （就是矩阵变化之后x点的值么）

* Matrix.y(x, y) 返回给定的Y坐标点变换后的矩阵描述 （就是矩阵变化之后y点的值么）

#Animation 

* Animation.delay(delay) 对一个已存在的动画增加延迟时间
```javascript
var anim = Raphael.animation({cx: 10, cy: 20}, 2e3);
circle1.animate(anim); // run the given animation immediately
circle2.animate(anim.delay(500)); // run the given animation after 500 ms
```

* Animation.repeat(repeat) 增加循环次数


# Set

* Set.clear() 从set里面清除全部元素

* Set.exclude(element) 从set里面排除某个元素

* Set.forEach(callback, thisArg) 循环这个set里面的元素进行callback操作

* Set.pop() 移出set里面的最后一个元素 并且 返回这个元素

* Set.push() 增加一个元素到set里面 

* Set.splice(index, count, [insertion…]) 移出某个元素

# eve  对事件的操作

* eve.off(name, f) 解除事件

*eve.on(name, f) 绑定事件  
这里不做过多介绍因为我也不懂这个