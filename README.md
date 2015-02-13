svg learning
===

## 1.svg 简介

1.1 svg特性
	矢量
	
1.2svg的使用方式

* 直接在浏览器打开
	ex:双击直接打开
* 在HTML中使用img标签引用
<pre>
	ex :
	![](https://github.com/semi-xi/svg-learn/raw/svg-learn/img/simple.svg) 
</pre>
* 直接在HTML中使用svg标签
	<pre>
	ex:<svg>xxxxxxxxx</svg>
	</pre>
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
