svg learning
===

##1.svg 简介

###1.1 svg特性

* 矢量
* 高大上

###1.2svg的使用方式

* 直接在浏览器打开
* 在HTML中使用img标签引用
* 直接在HTML中使用svg标签
* 作为css背景使用
<pre>
ex:<div class="div1"></div>
<style type="text/css">
	.div1{
		width:50px;
		height:50px;
		background-image: url(../img/simple.svg);
		background-size: 100%;
	}
</style>
</pre>

###1.3 基本的图形和属性
####基本图形

=
* 矩形rect
* 圆circle
* 椭圆ellipse
* 直线line
* 多段直线结合polyline
* 多边形polygon

####属性

=
* fill 填充
* stroke 边框
* stroke-width 边框宽度
* transform 变形 rotate 不需要带单位 ex rotate(5)


### 基本的api
* SVG_NS:http://www.w3.org/2000/svg 创建svg所有标签必备的一个东西 !important
* document.createElementNS(SVG_NS,ele) ele可以是svg line rect ellipse circle ploygon polyline等
* ele.appendChild(svgEle) 增加节点
* ele.setAttribute(name,value) 增加属性
* ele.getAttribute(name )获得属性

## 2.svg坐标部分

### 世界，视窗，视野

* svg width,视窗 代表这个svg的大小，渲染的区域大小
* svg的里面的内容代码矩形、曲线等等 定义这个世界
* viewBox 视野 观察这个世界的位置，在哪个位置去看这个世界 viewBbox =x y width height
* preserveAspectRatio 对世界的位置进行描述，填充策略


####  视窗与视野

=
一般来说视窗跟视野是一样大的。
但是可以认为的改变视野的大小
关于x,y 改变的情况
当改变x的时候，x越大，代表我在视觉点往右侧的方向走，这时候这个时候世界的内容自然相对于我们而言往左走
这样y变大的时候往上走就可以理解了。

=
关于width, height改变的情况
当宽度width变大的时候，可以理解为我们看到的东西更宽了，相对于世界在横向上缩小了。也就是我们可以看到一些原本在横方向上看不到的东西
当高度变大的时候，可以理解为我们看到的东西更高了，相对来说世界在竖直方向上缩小了，这样我们可以看到一些原本在竖直方向上看不到的东西。
例子的话可以看example part2

### 分组概念

用g标签来创建分组
他们的属性是可以继承的
可以嵌套
example part2/group


### 坐标系
* 用户坐标系 唯一的世界坐标系
* 自身坐标系 用于给自己定义形状，比如x,y坐标以及宽高，当发生了transform的时候本身的x，y，宽高是没有变的，变的是相对于前驱坐标系的一个坐标变换
 ex transform(50,50) 他只是相对于前驱坐标系移动了50 50像素 本身的属性并没有发生任何变化
* 前驱坐标系 ，就是父容器坐标系 相对于自己而言的上一级
* 参考坐标系 前面说过 当前驱坐标系发生变化的时候 实际上自身坐标系是没有发生变化的。但是我们也可以以前驱坐标系作为参考坐标系发生了某种变化
ex transform(50,50) 这时候自身坐标系的x跟y都是没变的 都是0,0 但是如果以世界坐标系作为参考 这样他的x,y应该是50,50
参考坐标系是使用其他坐标系来研究自身的情况使用的


### 线性变换

* rotate(xdeg)
* translate(x,y)
* scale(x,y)
* matrix(a,b,c,d,e,f)
PS： 矩阵的那个这里数学不好的可以跳过 使用极坐标的方法去表示矩阵

#### 坐标观察的方法
* .getBBox() 获取当前元素所占的矩形区域
* .getCTM() 获得视窗坐标系当当前元素自身坐标系的变化矩阵
* .getScreenCTM() 获得浏览器坐标系到当前元素自身坐标系的变换矩阵
* .getTransformToElement() 获得从指定元素的自身坐标系到当前元素的自身坐标系的变化矩阵

## 3.颜色、渐变和画刷

### rgb、hsl、透明度
都是css3支持的颜色表示方法

* rgb
红绿蓝 3个分量 取值[0,255]
显示器容易解析，但是不符合人类描述颜色的习惯,因为对于颜色的深浅需要同时改变几个值

* hsl
3个分量分别表示颜色 饱和度和亮度
h代表的是颜色([0,359]),s饱和度([0,100]),l亮度([0,100])
在hsl中 颜色表示色环的角度位置 0 为正12点方向
饱和度s代表的是颜色的鲜艳程度，0代表的是灰色
亮度l代表的是颜色的光亮程度0代表的是黑色,100代表的是白色.
详细可以参考exemple/part3
示例网站：http://paletton.com/ Hue表示色环的角度

* opacity
在透明度中 我们可以直接写 opactiy也可以写 rgba hsla [0,1]

### 渐变

* 线性渐变
linearGradient和stop 标签
方向 x1,y1,x2,y2
关键点位置(渐变过渡点)及颜色 offset  stop-color
gradientUnits 使用不同的坐标系来表示渐变的坐标点
默认是：objectBoundingBox 代表的是使用自身坐标系来表示渐变开始以及结束位置；如果用的是userSpaceOnUse 那么用的就是图形的世界坐标系
<code>
<svg xmlns="http://www.w3.org/2000/svg">
    <defs>
        <linearGradient id="grad1" gradientUnits="userSpaceOnUse" x1="100" y1="100" x2="150" y2="150">
            <stop offset="0" stop-color="#1497FC"/>
            <stop offset="0.5" stop-color="#A469BE"/>
            <stop offset="1" stop-color="#FF8C00"/>
        </linearGradient>
        <linearGradient id="grad2" gradientUnits="objectBoundingBox" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0" stop-color="#1497FC"/>
            <stop offset="0.5" stop-color="#A469BE"/>
            <stop offset="1" stop-color="#FF8C00"/>
        </linearGradient>
    </defs>
    <rect x="100" y="100" fill="url(#grad1)" width="200" height="150"/>
    <rect x="300" y="100" fill="url(#grad2)" width="200" height="150"/>
</svg>
</code>

说明：defs标签代表一个引用的集合,使用引用的方法是在图形的fill上使用url+(#+id) 代表在图形上使用某种渐变。渐变的必须都写在defs里面

* 径向渐变

同现行渐变 不过多了个焦点位置

<code>
	<svg xmlns="http://www.w3.org/2000/svg">
	    <defs>
	        <radialGradient id="grad2" cx="0.5" cy="0.5" r="0.5" fx="0.8" fy="0.2">
	            <stop offset="0" stop-color="rgb(20,151,252)" />
	            <stop offset="0.5" stop-color="rgb(164, 105, 190)" />
	            <stop offset="1" stop-color="rgb(255, 140, 0)" />
	        </radialGradient>
	    </defs>
	    <rect x="100" y="100" width="200" height="150" fill="url(#grad2)"></rect>
	</svg>
</code>

使用说明：
cx，cy代表开始的位置 r代表半径(r的最外面是结束区域) 默认采用自身 坐标系 也可以使用gradientUnits 改变使用不用的坐标系 如 userSpaceOnUse
cx、cy 和 r 属性定义外圈，而 fx 和 fy 定义内圈


### 笔刷
pattern 标签
patternUnits 默认是objectBoundingBox 和patternContentUnits属性 默认是userSpaceOnUse
patternUnits代表的是本身使用的一个坐标系统，pattrenContentUnits代表的是他的子元素使用的坐标系统。最好是都统一使用同样的值


## 4.路径path

###  概述
path由两部分组成 命令+参数 ，参数之间可以用空格或逗号隔开，有一种情况是例外的，就是负数，负数可以直接连在一起
ex:
<code>
	<path d="M0,0L10,20C30-10,40,20,100,100" stroke="red">
</code>
命令表 ：
* M/m (x,y)+  移动当前位置
* L/l (x,y)+  从当前位置绘制线段到指定位置
* H/h (x)+  从当前位置绘制⽔水平线到达指定的 x 坐标
* V/v (x)+  从当前位置绘制竖直线到达指定的 y 坐标
* Z/z   闭合当前路径
* C/c (x1,y1,x2,y2,x,y)+  从当前位置绘制三次⻉贝塞尔曲线到指定位置
* S/s (x2,y2,x,y)+  从当前位置光滑绘制三次贝塞尔曲线到指定位置
* Q/q (x1,y1,x,y)+  从当前位置绘制二次贝塞尔曲线到指定位置
* T/t (x,y)+  从当前位置绘制弧线到指定位置


ps: 命令表说明：
* 区分大小写：大写表示坐标参数为绝对位置，小写则为相对位置
* 最后的参数表示最终要到达的位置
* 上一个命令结束的位置就是下一个命令开始的位置
* 命令可以重复参数表示重复执行同一条命令

弧线命令(略显复杂)
A (rx, ry, xr, laf, sf, x, y) - 绘制弧线

参数说明：
* rx - （radius-x）弧线所在椭圆的 x 半轴长
* ry - （radius-y）弧线所在椭圆的 y 半轴长
* xr - （xAxis-rotation）弧线所在椭圆的长轴角度
* laf - （large-arc-flag）是否选择弧长较长的那一段弧 [0,1] 0代表短弧 1代表长弧
* sf - （sweep-flag）是否选择逆时针方向的那一段弧[0,1] 0代表逆时针 1代表顺时针
* x, y - 弧的终点位置

