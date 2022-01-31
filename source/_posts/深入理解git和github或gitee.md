---
title: 深入理解git和github或gitee
date: 2022-01-31 11:41:06
tags:
- git
- github
categories:
- git
---

深入理解Git和Github以及Gitee

<!--more-->

由于之前使用过github做博客，这里使用Gitee进行示范。

我们在另一台电脑打开一个Hijack8的Gitstudy项目，Git clone到本地。

![image-20220131100812243](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131100812243.png)

如图这个文件除了```.git```之外，就是我们的**工作区**，也就是些代码的地方。

本地除了工作区之外还有暂存区和本地仓库，暂存区存放的是你的改动，本地仓库记录你的历史版本。这两个区域都在```.git```文件夹中。

## Git clone下载源码

Git 支持两种方式下载源代码，一种是http方式，即找到仓库的http链接直接Git clone 链接即可。

还支持ssh下载源码。

**两种方式有什么区别呢？**

https方式对于初学者来说非常方便，可以直接进行克隆，而不需要配置ssh，但是https方式克隆的项目在进行fetch和push代码时都需要输入账户和密码，所以说比较麻烦，而ssh方式克隆的项目就不需要输入用户名密码。如果想使用ssh进行克隆，则必须要添加ssh key，所以说你必须是这个项目的拥有者或者管理员。

例如我在另一台电脑下载gitstudy项目

```
git clone git@gitee.com:hijack8.gitstudy
```

![image-20220131101728109](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131101728109.png)

发现有如上报错，这是为什么？

因为这里的git是使用ssh进行传输，而ssh传输在第一次需要进行验证。

在第一次使用git时候，我们需要先**设置姓名和邮箱地址**。

```
$ git config --global user.name "icemooo"
$ git config --global user.email "13356340251@163.com"
```

使用```git config --global --list ```可以查看自己的姓名邮箱是否配置。

注意一定要设置好自己的姓名邮箱，要不然自己提交的内容领导不会知道是你提交的。

设置好姓名和邮箱之后，我们开始配置ssh key

执行

```
ssh-keygen -t rsa -C "13356340251@163.com"
```

连续三次回车，在相应文件夹下生成了rsa文件。

![image-20220131104003759](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131104003759.png)

然后打开我们的码云，进入设置页的ssh key

把rsa pub文件复制过来生成新的ssh公钥。

![image-20220131104302260](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131104302260.png)

## Git pull更新本地代码

运行git pull可以将远端仓库的代码更新到本地。

```
git pull
```

![image-20220131110047378](%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3git%E5%92%8Cgithub%E6%88%96gitee/image-20220131110047378.png)

## Git add 

将内容添加到暂存区

## Git commit -m "messages"

```
git commit -m "3 files"
```

![image-20220131110832271](%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3git%E5%92%8Cgithub%E6%88%96gitee/image-20220131110832271.png)

这样就在本地git更新了此项目。

## git push

```
git push origin master
```

![image-20220131110945672](%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3git%E5%92%8Cgithub%E6%88%96gitee/image-20220131110945672.png)

推送到远端仓库中。

## git remote add origin <服务器>

如果我们的本地仓库并没有与远端仓库建立联系，可以这样来建立联系。

例如我们新建一个gitstudy文件夹，执行

```
git init
git remote add origin git@gitee.com:hijack8/gitstudy3.git
```

通过

`````
git remote -v
`````

来看远程仓库的关联。

![image-20220131111626386](%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3git%E5%92%8Cgithub%E6%88%96gitee/image-20220131111626386.png)

这时我们就可以进行pull push等操作了。

![image-20220131113827574](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131113827574.png)



通过

```
git remote remove origin
```

来断开连接。

