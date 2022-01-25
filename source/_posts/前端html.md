---
title: 前端html
date: 2022-01-16 15:58:09
tags: [”html“,“前端”]
categories: 
- 前端
- HTML
---

前端三件套之一 ：前端与html

这里总结了一些基础的内容，具体的内容可以在MDN官方网站查看：

https://developer.mozilla.org/zh-CN/

<!--more-->

## 前端技术栈

JS（行为） 用户响应

CSS（样式） 

HTML（内容） 页面结构内容

均运行在浏览器中，通过网络协议（HTTP）进行与服务器通信

## 前端关注

图形界面的人机交互

* 功能
* 美观
* 无障碍
* 安全
* 性能
* 兼容性 
* 体验

## 前端边界

nodejs 开发服务端应用

Web3D 

WebGL

## html

**HyperText   Markup  Language**

基本概念：

* 标签     例如`<p>``<img>``<html>`这种的就是标签。
* 属性     例如`<img src = "p.jpg" />`这里的src内容就是属性。

例子：

```html
<!doctype html>   版本
<html>
	<head>   编码 标题 不需要呈现给用户
		<meta charset = "utf-8">
		<titile>页面标题</title>
		<body>  这是页面内容 要呈现给用户
			<h1>一级标题</h1>
			<p>段落</p>
		</body>
	</head>
</html>
```

### DOM树

表示一个html文档各种标签内容的树形结构。

![image-20220116125534942](%E5%89%8D%E7%AB%AFhtml/image-20220116125534942.png)

### html语法

* 标签和属性不区分大小写
* 空标签可以不闭合 不如input meta
* 属性值**推荐**用“”包裹
* 某些属性值可以省略 比如写成required

**标题** 

h1~h6

**列表**

* 有序列表
* 无序列表
* 自定义列表

```html
<ol>有序列表
	<li>大帅</li> 
	<li>大莲</li>
</ol>

<ul>无序列表
	<li>大帅</li> 
	<li>大莲</li>
</ul>

<dl>自定义列表
	<dt>大帅</dt>
	<dd>大莲</dd>
	<dt>大莲</dt>
	<dd>大莲</dd>
	<dd>大莲</dd>
	<dd>大莲</dd>
</dl>
```

![image-20220116142444909](%E5%89%8D%E7%AB%AFhtml/image-20220116142444909.png)

**链接**

* 超链接 

属性href中表明超链接地址，例如`<a href = "http://www.baidu.com">`

属性target一般使用`target=blank` 表示新建一个页面显示内容。

* img

两个必要属性：

1. src 是图片的URL
2. alt 是如果没有显示出图片的话他会显示什么

* audio 音频

1. src URL
2. 一般写一个`controls`表明使用默认的播放器

* video

与audio类似

```html
<a href = "http://www.baidu.com">
    百度官网
</a>
<br>
<a href = "http://www.baidu.com"
   target = "_blank">
    百度官网2
</a>
<br>
<img 
     src = "haocun.jpeg" 
     alt = "Metal movable type"        
     width = "400"> 
<br>
<audio
       src = "Dream_It_Possible.flac"
       controls          <!--默认显示浏览器默认播放控件-->
></audio>
<br>
<video       
	src = "c1c00f3a0c1f7692c16f1264b1236376.mp4"
	controls  
	width = "400"        
></video>
```

![image-20220116150358737](%E5%89%8D%E7%AB%AFhtml/image-20220116150358737.png)

**输入**

可以输入

使用input

* 默认为一个小输入框 可以通过placeholder设置默认的字段
* `<input type = "range">`拖动条
* `<input type = "number" min = "1" max = "10">`输入数字
* `<input type = "date" min = 2018-02-01>`输入日期

使用textarea 输入一段话

```html
<input placeholder = "请输入用户名"> 
<input type = "range">
<input type = "number" min = "1" max = "10">
<input type = "date" min = 2018-02-01>
<textarea>Hey</textarea>
<!--去掉换行之后-->
```

![image-20220116150938510](%E5%89%8D%E7%AB%AFhtml/image-20220116150938510.png)

至于怎么处理这些输入的数据，我们在js中就会学到。

 **输入进阶--选择**

* input的checkbox 复选框

```html
<p>
	<label><input type = "checkbox" name = "sport"/>
		A</label>
	<label><input type = "checkbox" name = "sport" />
		B</label>
</p>
```

![image-20220116153054102](%E5%89%8D%E7%AB%AFhtml/image-20220116153054102.png)

* input的radio单选框

```html
<p>
	<label><input type = "radio" name = "food"/>
		C</label>
	<label><input type = "radio" name = "food" />
		D</label>
</p>
```

![image-20220116153123226](%E5%89%8D%E7%AB%AFhtml/image-20220116153123226.png)

* select 下拉选项

```html
<select>
    <option>A</option>
	<option>B</option>
	<option>C</option>
	<option>D</option>
</select>
```

![image-20220116153324034](%E5%89%8D%E7%AB%AFhtml/image-20220116153324034.png)

* 可以有默认选项的输入框

```html
<input list = "countries"/>
<datalist id = "countries">
    <option>A</option>
    <option>B</option>
</datalist>
```

![image-20220116153338555](%E5%89%8D%E7%AB%AFhtml/image-20220116153338555.png)

**文本类**

* blockquote 定义块引用 内部是引用的内容

```html
<blockquote cite = "http://">
	<p><blockquote> 标签定义块引用。
		blockquote 之间的所有文本都会从常规文本中分离出来，经常会在左、右两边进行缩进（增加外边距），而且有时会使用斜体。也就是说，块引用拥有它们自己的空间。</p>
	</blockquote>
```

![image-20220116153842374](%E5%89%8D%E7%AB%AFhtml/image-20220116153842374.png)

* cite 一般可以表示作品的名字

```html
<p>我最喜欢的书是<cite>《小王子》</cite></p>
```

* q一般可以引用一句话

```html
<p>陶渊明说过<q>什么什么</q></p>
```

![image-20220116154135448](%E5%89%8D%E7%AB%AFhtml/image-20220116154135448.png)

* code在段落中引用代码

```html
<p>	当我们使用<code>const</code>的时候</p>
```

![image-20220116154303859](%E5%89%8D%E7%AB%AFhtml/image-20220116154303859.png)

* pre + code引用代码块

```html
<pre>
<code>
代码段
</code>
</pre>
```

* strong 突出   重要紧急的一句话

* em 表示语气上的强调

```html
	<strong>我的世界，你最耀眼</strong>是他的个性签名。
	他们说这是条狗，其实这是一只<em>猫</em>
```

![image-20220116154952571](%E5%89%8D%E7%AB%AFhtml/image-20220116154952571.png)

### 内容划分

一个html文档可以分为如下几个方面内容。

* header  头部 这里跟head标签是不同的
* main     主体部分 一般一个html文档中只有一个main部分
* aside    这是跟主体部分不相关的部分，例如广告等
* footer    底部的版权等

![image-20220116132005671](%E5%89%8D%E7%AB%AFhtml/image-20220116132005671.png)

```html
<header></header>
<main>
	<article>
        
    </article>
    <article>
    </article>
</main>  重点
<aside></aside> 相关的内容
<footer></footer> 尾部标签
```

### 语义化

* html中的**元素**、**属性**及**属性值**都用于某些含义
* 开发者应该遵循**语义**
  * 有序列表用  无序列表用
  * lang属性表示内容使用的语言
* html是传递的内容而不是样式

对于语义化的理解：

该使用什么标签就使用什么标签，比如这是一个二级标题就需要使用`<h2>`而不是调整字体样式让他看起来像二级标题。



## 补充与注意

* `<div> <span>`

div占用的是一行

span是占用要多宽有多宽的空间

* class = header  => header

很多html5之前的版本使用类似的表达方式即class = header ，这里建议直接使用header

* template 是一个模板 用来自定义标签
* **跨域**？跨域就是某些场景里面只能使用本域的资源而不能使用别的域的资源 ，例如a标签 img可以跨域 ，但是https页面中不能直接引http的img 他会破坏安全性
* iframe 不需要交互时候   直接嵌入第三方内容
* 用div做按钮 加额外的标签  可以直接用button
* 组件库注意语义化
* h5离线存储 localstorage本地存储  控制请求没有网 的时候显示 serviceworker
* 每一段是用p 每一块内容可以用section
* meta标签用到查就行 移动端用的多
* 行内样式：样式和内容分离 一般不要在标签里面写style 组件化开发 强调关注带你分离 内容行为样式放在一个地方 style属性

## 附上代码

```html
<ol>有序列表
	<li>大帅</li> 
	<li>大莲</li>
</ol>

<ul>无序列表
	<li>大帅</li> 
	<li>大莲</li>
</ul>

<dl>自定义列表
	<dt>大帅</dt>
	<dd>大莲</dd>
	<dt>大莲</dt>
	<dd>大莲</dd>
	<dd>大莲</dd>
	<dd>大莲</dd>
</dl>
<a href = "http://www.baidu.com">
    百度官网
</a>
<br>
<a href = "http://www.baidu.com"
   target = "_blank">
    百度官网2
</a>
<br>
<a>
<img 
     src = "haocun.jpeg" 
     alt = "Metal movable type"        
     width = "400"> 
</a>
<br>
<audio
       src = "Dream_It_Possible.flac"
       controls          默认显示浏览器默认播放控件
></audio>
<br>
<video       
	src = "c1c00f3a0c1f7692c16f1264b1236376.mp4"
	controls  
	width = "400"        
></video>

<br>
<br>
<br>
<br>
<input placeholder = "请输入用户名">  
<br>
<input type = "range">
<br>
<input type = "number" min = "1" max = "10">
<br>
<input type = "date" min = 2018-02-01>
<br>
<textarea>Hey</textarea>
<br>
<p>
	<label><input type = "checkbox" name = "sport"/>
		A</label>
	<label><input type = "checkbox" name = "sport" />
		B</label>
</p>
<p>
	<label><input type = "radio" name = "food"/>
		C</label>
	<label><input type = "radio" name = "food" />
		D</label>
</p>
<br>
<br>
<br>
<select>
    <option>A</option>
	<option>B</option>
	<option>C</option>
	<option>D</option>
</select>
<br>
<br>
<br>
<input list = "countries"/>
<datalist id = "countries">
    <option>A</option>
    <option>B</option>
</datalist>
<br>
<br>
<blockquote cite = "http://">
	<p><blockquote> 标签定义块引用。
		blockquote 之间的所有文本都会从常规文本中分离出来，经常会在左、右两边进行缩进（增加外边距），而且有时会使用斜体。也就是说，块引用拥有它们自己的空间。</p>
	</blockquote>
	<p>我最喜欢的书是<cite>《小王子》</cite></p>
	<p>陶渊明说过<q>什么什么</q></p>
	<p>
		当我们使用<code>const</code>的时候
	</p>
	<pre>
		<code>

		</code>
		</pre>
	<strong>我的世界，你最耀眼</strong>是他的个性签名。
	他们说这是条狗，其实这是一只<em>猫</em>  
```

