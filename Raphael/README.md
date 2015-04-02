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
  *  > 或者easeOut 或者ease-out 先加速再减速
 *  <> 或者easeInOut 或者 ease-in-out
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
``` jvascript
    var t = paper.text(50, 50, "Raphaël\nkicks\nbutt!");
```

* Paper.top 找到表现上最上面的那个元素 与Paper.bottom相反

