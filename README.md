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
ex:
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

* 矩形rect
* 圆circle
* 椭圆ellipse
* 直线line
* 多段直线结合polyline
* 多边形polygon

####属性

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

### 世界,视窗,视野

* svg width,视窗 代表这个svg的大小,渲染的区域大小
* svg的里面的内容代码矩形、曲线等等 定义这个世界
* viewBox 视野 观察这个世界的位置,在哪个位置去看这个世界 viewBbox =x y width height
* preserveAspectRatio 对世界的位置进行描述,填充策略

preserveAspectRatio(aligin,meet/slide)
aligin:
xMid xMin xMax
YMid YMin YMax
两者组合成第1个参数 ex xMidYMid
####  视窗与视野

<!-- =
一般来说视窗跟视野是一样大的。
但是可以人为的改变视野的大小
关于x,y 改变的情况
当改变x的时候,x越大,代表我在视觉点往右侧的方向走,这时候这个时候世界的内容自然相对于我们而言往左走
这样y变大的时候往上走就可以理解了。

=
关于width, height改变的情况
当宽度width变大的时候,可以理解为我们看到的东西更宽了,相对于世界在横向上缩小了。也就是我们可以看到一些原本在横方向上看不到的东西
当高度变大的时候,可以理解为我们看到的东西更高了,相对来说世界在竖直方向上缩小了,这样我们可以看到一些原本在竖直方向上看不到的东西。
例子的话可以看example part2 -->

解释：视野就是在svg中根据x,y,width,height开辟出来的一块区域,并且把他放大到整个svg区域。根据preserveAspectRatio 属性的不同,对这块位置进行不同的填充策略,也就是显示方式以及对齐方式


### 分组概念

用g标签来创建分组
他们的属性是可以继承的
可以嵌套
example part2/group


### 坐标系
* 用户坐标系 唯一的世界坐标系
* 自身坐标系 用于给自己定义形状,比如x,y坐标以及宽高,当发生了transform的时候本身的x,y,宽高是没有变的,变的是相对于前驱坐标系的一个坐标变换
 ex transform(50,50) 他只是相对于前驱坐标系移动了50 50像素 本身的属性并没有发生任何变化
* 前驱坐标系 ,就是父容器坐标系 相对于自己而言的上一级
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
显示器容易解析,但是不符合人类描述颜色的习惯,因为对于颜色的深浅需要同时改变几个值

* hsl
3个分量分别表示颜色 饱和度和亮度
h代表的是颜色([0,359]),s饱和度([0,100]),l亮度([0,100])
在hsl中 颜色表示色环的角度位置 0 为正12点方向
饱和度s代表的是颜色的鲜艳程度,0代表的是灰色
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


说明：defs标签代表一个引用的集合,使用引用的方法是在图形的fill上使用url+(#+id) 代表在图形上使用某种渐变。渐变的必须都写在defs里面

* 径向渐变

同现行渐变 不过多了个焦点位置



使用说明：
cx,cy代表开始的位置 r代表半径(r的最外面是结束区域) 默认采用自身 坐标系 也可以使用gradientUnits 改变使用不用的坐标系 如 userSpaceOnUse
cx、cy 和 r 属性定义外圈,而 fx 和 fy 定义内圈


### 笔刷
pattern 标签
patternUnits 默认是objectBoundingBox 和patternContentUnits属性 默认是userSpaceOnUse
patternUnits代表的是本身使用的一个坐标系统,pattrenContentUnits代表的是他的子元素使用的坐标系统。最好是都统一使用同样的值


## 4.路径path

###  概述
path由两部分组成 命令+参数 ,参数之间可以用空格或逗号隔开,有一种情况是例外的,就是负数,负数可以直接连在一起

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
* 区分大小写：大写表示坐标参数为绝对位置,小写则为相对位置
* 最后的参数表示最终要到达的位置
* 上一个命令结束的位置就是下一个命令开始的位置
* 命令可以重复参数表示重复执行同一条命令

弧线命令(略显复杂)
A (rx, ry, xr, laf, sf, x, y) - 绘制弧线

参数说明：
* rx - （radius-x）弧线半长轴长度
* ry - （radius-y）弧线半短轴长度
* xr - （xAxis-rotation）弧线所在椭圆的长轴与水平线的角度
* laf - （large-arc-flag）是否选择弧长较长的那一段弧 [0,1] 0代表短弧 1代表长弧
* sf - （sweep-flag）是否选择逆时针方向的那一段弧[0,1] 0代表逆时针 1代表顺时针
* x, y - 弧的终点位置

贝塞尔曲线
CSQT(被人称为厕所切图)大写表示绝对位置 小写代表相对自己的位置
* C三次贝塞尔曲线
* S 三次光滑贝塞尔曲线
* Q二次贝塞尔曲线
* T二次光滑贝塞尔曲线

PS： 光滑代表的是可以前一个曲线跟后一个曲线收尾相连。
如果是二次贝塞尔曲线,那么最后一个点是这个贝塞尔曲线的起始点
如果是三次贝塞尔曲线,那么最后一个点的镜像点作为这个点的第1个控制点这样我们可以就可以省略写一个参数

## 5.文字
### text、textspan 标签
textspan是text的子元素

属性：
* x: 文字左下角对齐的x坐标
* y: 文字左下角对齐的y坐标 跟基线位置对齐(中文)
* dx： 文字在横向上的偏移,多个变量用空格隔开,表示对多个字符进行偏移控制
* dy: 同dx,差异在于dy是对文字在垂直方向上的控制
* style : 设置样式
* text-anchor : 水平居中
* dominant-baseline ： 垂直居中（没啥用,应该说出来的不是你想要的）

requestAnimationFram(fn) //动画

如何模拟垂直居中
对于英文 是对齐基线的。 现在说的是中文的情况:
利用getBBox() 获取好getBBox().y与跟getBBox().height 以及text的y实现

### 路径文本textpath
textpath xlink:href 属性 路径
textpath startOffset 起始点的位置 百分比
text x属性代表文字在路径上的偏移量,小于0的时候代表最左边的文字会消失
text y属性 无效
text text-anchor 文本的位置对齐与起始点 start middle end

ps： 创建textpath的时候需要大写 document.createElementNS('ns','textPath')


### 超链接 a
属性
xlink:href  链接地址
xlink:title 提示
target  打开的方式_blank / _self

##  6.图形的引用、裁切和蒙版
* use  标签,利用属性xlink:href=#id 引用图形
* clip-path 作为一个属性 clip-path=url(#id)
* mask 标签定义蒙版,mask属性引用蒙版 maxk=url(#moon-mask)

### 蒙版

蒙版的颜色只有2种颜色 黑、白 黑色代表透明区域透明,白色代表显示。
看例子example/part6


## 7.动画

标签:
* set 不能触发连续动画,只能在特定时间修改某个属性值
* animate 基础动画 实现单属性过渡效果
* animateColor 已废
* animateTransform transform变换
* animateMotion 路径
ps： 可以多个标签自由组合


属性 :
* attributeName 变化的元素属性名称 可以是css属性:opacity 或者是标签本身的属性x,y 等等
* attributeType  表明attributeName的属性列表属于什么。值:css/xml/auto ,对于x,y属于xml,传统的css属性则属于css,也可以让浏览器自己判断,不确定的时候可以不写,让浏览器自己判断
* form,to,by,values
    * form 起始值
    * to 结束值
    * by 动画的相对变化值
    * values 用分号分隔的一个或多个值
    * Ps：
        * 起始值是默认值。则form可以忽略
        * to,by最少出现一个,区别是一个绝对,一个相对 to=100 某值变化到100  by=100 在原来的值基础上变化多100
        * to,by同时出现,y没用
        * to,by,value  如果都不写的话则没效果
        * value 出现的时候 form to by 都忽略 ,用于实现多个关键帧的情况
* begin end 动画的开始时间跟结束时间 begin 可以设置一组竖直 表示的是几秒之后动画走一下,几秒之后再走一下(没走完,会立刻重新开始开始) (表示不理解)
* 时间
    * 常用h/min/s/ms;还可以1.5s 01:30 1.5 无单位默认s
    * 也可以用[元素id].begin/end +/-时间 例如 x.end
    * 也可以用偏移  直接在数值前面+/- ,x.end-1s
    * 也可以与事件关联 [元素的id].[事件类型] +/- 时间值  参考example/part7 事件的类型跟传统的一样可以是click,mouseover,mouseenter,mouseout 等等
    * 也可以指定repeat-value 重复多少次之后干嘛 [元素的id].repeat(整数) +/- 时间值  begin="x.repeat(2)"
    * accesskey-value  按某个键之后动画开始 begin="accessKey(keyname ex:s)"  目前只有火狐兼容(2015.03.10)
    * indefinite begin="indefinite" 需要用js去触发 选定这个animate click这个svg或者什么的时候触发animate.beginElement()或者这样a  xlink:href="#animate"; 这样就会有动画,貌似跟事件那个差不多
* dur 持续时间 常规时间值/indefinite indefinite指时间无限,实际上是动画不执行
* calcMode、keyTimes、keySplines
    * calcMode
        * discrete from值直接跳到to值。
        * linear 默认值 value值之前的变化速率是一致的ex values="200;300;600"
        * paced  定义了贯穿整个动画变化产生平稳步调的写入方式。仅支持线性数值区域内的值，这样点之间“距离”的概念才能被计算,当cakcMode值为paced时,keyTimes、keySplines无效
        * spline 插值的贝塞尔曲线 。spline的点定义在keyTimes属性中，每个时间间隔控制点由ketSpline定义
    * keyTimes 分号间隔的一组值，名字上是关键时间点的意思。如果有values属性，那么keyTimes的值得数目要与values的值得数目一致,
    * keySplines 表示耳朵是与KeyTimes相关联的一组贝塞尔控制点(默认是0 0 1 1),控制点使用4个浮点值x1 y1 x2 y2 只有spline参数的时候才有用 范围值0-1,总是比keyTimes少1个值
    * 说明：动画,实际上是values,keyTimes,keySplines。values确定动画的关键位置,keyTimes确定到这个关键点需要的时间,keySplines确定的是每个时间点段之间的贝塞尔曲线,也就是具体的缓动表现,我们平时CSS3写的transition动画效果,也是这么回事,这是values值就两个,所以,keyTimes只能是0;1, 贝塞尔曲线就只有一个,要不ease, 要不linear等
* repeatCount,repeatDur 动画执行总次数/重复动画的总时间 可以是普通时间或者是indefinite
* fill 动画间隙的填充方式  freeze/remove  。 remove是默认值 表示动画结束之后直接回到开始的地方。freeze表示动画结束之后保持动画结束之后的状态
* accumulate,additive
    * accumulate 可选none/sum 默认为none  如果是sum，表示动画结束时候的位置作为下次动画的起始位置
    * additive 是否附加 可选 replace/sum  默认是replace 如果值为sum表示动画的基础知识会附加到其他低优先级的动画上,可以理解为叠加效果,尤其出现在相同attributeName的动画上
* restart   可选always/whenNoActive/never 默认值always。在触发式(点击等)动画的时候,我们只希望他执行一次或者在执行中不允许再次被触发这样这个值就有用了。 always表示只要事件触发了就重来,whenNoActive 表示在进行动画的时候触发不会重新开始,直到结束才可以,never表示只会触发一次.
* ...
