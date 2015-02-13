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
* transform 变形