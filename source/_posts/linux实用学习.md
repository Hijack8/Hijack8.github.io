---
title: linux实用学习
date: 2022-01-31 17:48:02
tags:
- linux
categories:
- linux
---

## linux文件类型

### 1 普通文件

例如txt文件等。

利用`ls -lh`命令查看文件属性，发现对应属性为 **-**

### 2 目录文件

属性为**d**

<!--more-->

### 3 设备文件

#### 块设备文件

属性为**b**

例如dev文件夹下的sda文件，表示SCSI硬盘，hda表示IDE硬盘。

![image-20220131175420142](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131175420142.png)

#### 字符设备文件

属性为**c**

例如dev下的null文件，任何送入这个设备的内容都会被忽略。

![image-20220131175327512](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131175327512.png)

### 4 管道文件

属性为p

也叫作FIFO文件。

### 5 链接文件

#### 硬链接文件

硬链接文件是已存在文件的另一个文件，对硬链接文件进行操作时，结果和软链接相同，如果删除硬链接文件的源文件，硬链接文件仍然存在，而且保留了原有的内容。

#### 软链接文件

又叫符号链接文件，类似快捷方式，对这个文件进行操作时，系统会自动转换为对源文件的操作。删除此文件不会删除源文件本身。

属性为l   ->  link

## 链接文件 --- 共享文件

分为硬链接和软链接

###  硬链接

不会重新分配inode，公用inode。

不能给目录硬链接。

只能在同一个文件系统下建立。

### 软链接

包含了另一个文件的路径名，可以是任意文件或目录，可以链接不同文件系统的文件，和Windows下的快捷方式类似。

### ```ln```命令创建链接文件

```
ln -s 源文件 链接文件    ---- 软链接
ln -d 源文件 链接文件
```

### 硬链接实验

![image-20220131194142784](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131194142784.png)

![image-20220131194355272](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131194355272.png)

两个硬链接文件的inode是一致的。

改变a的内容，b的内容也跟着改变。

![image-20220131194722149](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131194722149.png)

删除a，发现b的link数减为1。

![image-20220131194839938](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131194839938.png)

### 软链接实验

新建a ，b为a的软链接

![image-20220131195244891](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131195244891.png)

![image-20220131195316516](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131195316516.png)

可以看到软链接确实类似快捷方式。

![image-20220131195359017](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131195359017.png)

说明b的源文件还是a，链接数还是1。

![image-20220131195508443](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131195508443.png)

二者inode也不同。

![image-20220131195624305](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131195624305.png)

删除源文件，发现软链接也无法访问。

### 硬链接文件复制了源文件的inode，二者本质上可以看成是一样的文件，有同等的地位，删除其中一个，系统不会删去inode。

### 软链接文件仅仅是一个具有inode号的一个新文件，文件中存储着另一个文件的路径名。

## Shell编程

### 简单的例子

```shell
# !/bin/bash
#filename:date
echo "Mr.$USER,Today is:"
echo 'date'
echo Whish you a lucky day!
```

![image-20220131205510102](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131205510102.png)

## 用户账户

* root用户

  UID为0

* 系统用户

  系统自身必不可少的用户，比如bin、ftp、email 。UID为1~999

* 普通用户

  可以登录系统，在linux系统上进行普通操作，UID为1000~。。。

### /etc/passwd 文件

linux中所有用户都记录在这个文件中

![image-20220131214308209](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131214308209.png)

  

  这就是我当前用户的信息

这个x是密码，实际上被映射到shadow文件中。

### etc/shadow文件

![image-20220131214602258](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131214602258.png)

这就是加密的密码

## 创建用户

创建zhangsan

```shell
useradd zhangsan
```

![image-20220131215023447](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131215023447.png)

改密码

![image-20220131215143098](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131215143098.png)

创建属于root成员的pp用户

```
useradd -g root pp
```

![image-20220131215433962](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131215433962.png)

可以看到用户的GID字段为0，表示为root群组。

## 组群账户

### /etc/group

![image-20220131220247808](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220131220247808.png)
