svg learning
===

## 1.svg 简介

1.1 svg特性
	矢量
1.2svg的使用方式

** 直接在浏览器打开
	ex:双击直接打开
** 在HTML中使用<img>标签引用
	ex :<img src="img/simple.svg" alt="">
** 直接在HTML中使用svg标签
	ex:<svg>xxxxxxxxx</svg>
** 作为css背景使用
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
