---
title: 深入理解git--branch
date: 2022-01-31 14:29:50
tags:
- git
- branch
categories:
- git
---

对git branch进行实验从而深入理解。

## ```git checkout -b <branch_name>```新建分支

```
git checkout -b branch_test1
```

![image-20220131143345502](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131143345502.png)

可以看到我们的分支变为了branch_test1分支。

<!--more-->

## ```git checkout master```切换为主分支

```
git checkout master
```

切换某个分支。

## `git branch -d <branch_name>`将分支删除

![image-20220131143648498](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131143648498.png)



## `git branch `查看分支

![image-20220131143826860](https://gitee.com/Hijack8/tc/raw/master/img/image-20220131143826860.png)



## `git merge <branch_name>`将任何分支合并到主分支中去



## 然后我们进行实际实验。。。。

在另一台主机中对master分支进行修改，对test1.md文件添加内容，然后push上去，注意这里的**两个主机必须都是这个项目的开发者或者是拥有者才有权限进行push操作。**

![image-20220131144944427](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131144944427.png)

这样成功push后可以在远端发现变化。

![image-20220131145049374](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131145049374.png)

我们新建一个分支，并转入这个分支。

![image-20220131145644710](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131145644710.png)

然后对这个分支进行修改操作。删除test2

![image-20220131145747933](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131145747933.png)

然后切换为主分支查看：

![image-20220131145958268](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131145958268.png)



发现很不幸，主分支上的文件也被删除了。这是因为没有进行正常的add commit操作。

我们重新回到branch_a分支，进行删除文件操作。

![image-20220131150203460](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131150203460.png)

这时git进行了两次提交，我们回到master分支。

![image-20220131150256035](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131150256035.png)

发现master分支内的文件并没有被删除，而是仅仅branch_a分支中的文件被删除了两个。

进一步我们进行git merge操作

![image-20220131150642575](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131150642575.png)

发现成功将branch_a分支中的改动合并到主分支中去。

删除分支git checkout -d

![image-20220131150805068](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131150805068.png)

这就是分支在本地的操作，**如果我们不把分支push到远端，这个新建的branch_a分支就只有天知地知你知我知**。

## 合并冲突

经常会有合并冲突的问题。

例如，在branch_a中改了test1，在master中也改了test1，就会冲突。

我们尝试一下。

![image-20220131151609812](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131151609812.png)

```
git diff
git add .
git commit -m "conflict"
git merge branch_b
```

利用git diff 显示差异内容，然后add commit提交，然后merge合并即可。

