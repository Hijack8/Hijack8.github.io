---
title: Typora写Markdown文件利用图床
date: 2022-01-30 21:02:49
tags:
- Typora
- 图床
- Markdown
categories:
- Typora
---

本文用来探索一个免费的图床来实现Typora笔记中图片的上传。

<!--more-->

## 下载picgo软件

注意Typora中有一个好用的直接上传图片的软件叫picgo，可以通过右键直接将typora中的文件上传。

![image-20220130213638585](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130213638585.png)

这里找到偏好设置中的下载PicGoAPP选项，

![image-20220130213812075](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130213812075.png)

点击免费下载

![image-20220130213835876](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130213835876.png)

选择一个稳定的版本比如2.3.0

![image-20220130213914823](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130213914823.png)

下载这个版本即可

![image-20220130213947934](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130213947934.png)

下载完成后在偏好设置中配置好picgo位置

这样picgo就配置好了。

## 配置Gitee插件

有很多地方可以作为图床，可以使用自己的服务器，也可以使用Github，Gitee这样的平台。

现在picgo已经集成了Github和Gitee的插件，我们只需要打开picgo，然后搜索gitee插件

![image-20220130214317847](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214317847.png)

直接下载这个插件，在图床设置中打开Gitee图床

![image-20220130214454423](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214454423.png)

在这里面需要输入三个信息，owner是你的Gitee用户名，repo是你新建的Gitee图床仓库，token是生成的私人令牌，这里仅介绍令牌如何生成。

![image-20220130214741550](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214741550.png)





在码云账户中选择个人主页

![image-20220130214809553](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214809553.png)

选择个人设置

![image-20220130214828966](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214828966.png)

选择私人令牌，生成令牌即可。

## 如何使用

在平时记录笔记的时候可以将图片直接保存到本地，这样是最稳妥的，所以可以这样设置

![image-20220130214953350](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130214953350.png)

这样首选就是保存到本地一个相同路径的同名文件夹中。

然后我们如果想上传这个图片直接右键上传图片即可。

![image-20220130215319056](https://gitee.com/Hijack8/tc/raw/master/img/image-20220130215319056.png)

可以发现我们的码云仓库确实上传了一堆图片。

