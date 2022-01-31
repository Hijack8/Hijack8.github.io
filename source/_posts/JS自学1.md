---
title: JS自学1
date: 2022-01-25 23:48:35
tags:
- JavaScript
- 前端
categories:
- 前端
- JS自学笔记
---

JavaScript是前端知识体系中最为重要的一个底层语言，我学习了许多前端课程之后，发现自己JavaScript的基础太差，导致很多课程跟不上趟，所以只能越拉越多，以至于我痛下决心要全面地学习一下JavaScript。这里是学习的尚硅谷课程的JavaScript课程记录的笔记。

<!--more-->

## JavaScript简介

JavaScript初始是用来处理网页中的前端验证。即不需要使用服务器进行验证，直接使用浏览器进行验证。

由于JavaScript开发的公司之前开发过Java语言，所以说这个公司直接取名为JavaScript。

ECMAScript是JavaScript现在使用的统一标准。这个标准由各个厂商进行实现。

不同的浏览器厂商会对该标准由不同的实现。

Chrome的v8引擎是最快的。

## JavaScript由三个部分

* ECMAScript
* DOM
* BOM

## JavaScript的特点

* 解释型语言，不用编译直接运行。
* 动态语言，变量比较任意。
* 基于原型的面向对象。

## js代码写在哪儿

js代码需要编写在script标签中。

## 基本代码-helloworld

```html
<head>
    <meta charset="utf-8">
    <script>
        //输出警示框
        alert("hello world");
        //向body输出
        document.write("hello");
        //向控制台输出
        console.log("hello");
    </script>
</head>
<body>

</body>
```

* JavaScript是从上到下顺序执行的。

## js代码所有位置

* 可以写到标签的onclick属性中。

  当我们点击按钮时候，js才会执行。

  每点一次执行一次

* 写到超链接的herf属性中

* 写到script标签中。

* 外部文件，写到外部js文件中，通过script标签引入`<script src = "hello.js"></srcipt>`

  可以在不同的页面中同事引用，可以利用到浏览器的缓存机制。**推荐使用**

  srcipt标签一旦用于引入外部文件，就**不能再编写代码**了。

```html
<head>
    <meta charset="utf-8">
   	<!--
		从外部文件引入js
		-->
    <script src = "hello.js"></script>
    <script>
        //输出警示框
        alert("hello world");
        //向body输出
        document.write("hello");
        //向控制台输出
        console.log("hello");
    </script>
</head>
<body>
    <!--
        注意这里js代码写入标签的onclick属性中
        当点击按钮时候才会执行
        这里alert的内容必须使用单引号
    、-->
    <button onclick="alert('hhhh');">点我</button>
    <!---
        可以写在超链接的href属性中
    -->
    <a href="javascript:alert('666');">还有我</a>
</body>
```

