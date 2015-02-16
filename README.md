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
* 矩形<rect>
* 圆<circle>
* 椭圆<ellipse>
* 直线<line>
* 多段直线结合<polyline>
* 多边形<polygon>

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

用g标签来创建分组<br>
他们的属性是可以继承的<br>
可以嵌套<br>
example part2/group


### 坐标系
* 用户坐标系 唯一的世界坐标系
* 自身坐标系 用于给自己定义形状，比如x,y坐标以及宽高，当发生了transform的时候本身的x，y，宽高是没有变的，变的是相对于前驱坐标系的一个坐标变换<br>
 ex transform(50,50) 他只是相对于前驱坐标系移动了50 50像素 本身的属性并没有发生任何变化
* 前驱坐标系 ，就是父容器坐标系 相对于自己而言的上一级
* 参考坐标系 前面说过 当前驱坐标系发生变化的时候 实际上自身坐标系是没有发生变化的。但是我们也可以以前驱坐标系作为参考坐标系发生了某种变化<br>
ex transform(50,50) 这时候自身坐标系的x跟y都是没变的 都是0,0 但是如果以世界坐标系作为参考 这样他的x,y应该是50,50 <br>
参考坐标系是使用其他坐标系来研究自身的情况使用的


### 线性变换

* rotate(xdeg)
* translate(x,y)
* scale(x,y)
* matrix(a,b,c,d,e,f)
PS： 矩阵的那个这里数学不好的可以跳过 使用极坐标的方法去表示矩阵<br>

#### 坐标观察的方法
* .getBBox() 获取当前元素所占的矩形区域
* .getCTM() 获得视窗坐标系当当前元素自身坐标系的变化矩阵
* .getScreenCTM() 获得浏览器坐标系到当前元素自身坐标系的变换矩阵
* .getTransformToElement() 获得从指定元素的自身坐标系到当前元素的自身坐标系的变化矩阵