---
title: linux实用学习2
date: 2022-02-01 08:33:52
tags:
- linux
categories:
- linux
---

## linux磁盘分区

输入

```
fdisk /dev/sda
```

进入fdisk界面，显示磁盘分区信息。

![image-20220201084206440](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201084206440.png)

图中sda1是引导分区

blocks表示分区容量，默认一块为1KB

<!--more-->

## linux文件系统

对一个新的磁盘进行分区之后，要对这些分区进行格式化并创建文件系统，一个分区只有建立了文件系统才能使用。

两个主流文件系统：

* XFS
* ext4

我们使用命令

```
mkfs -v /dev/sda2
```

来查看分区的file system信息。

![image-20220201085237715](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201085237715.png)

可以看到sda1为引导盘。

使用命令

```
df 
```

查看文件系统的挂载情况。

![image-20220201090217010](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201090217010.png)

所谓挂载，就是将某个硬件设备实际放在某个文件夹去访问。

## tar文件

* 压缩文件夹 

```
tar cvf hello.tar hello 
```

![image-20220201091645444](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201091645444.png)

![image-20220201091741181](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201091741181.png)

* 解压缩文件

```
tar xvf hello.tar jieya
```

![image-20220201091912557](https://gitee.com/Hijack8/tc/raw/master/img2/image-20220201091912557.png)

* 压缩为gzip文件

```
tar zcvf hello.tar.gz hello
```

* 解压缩gzip文件

```
tar zxvf hello.tar.gz
```

* 压缩为bzip2

```
tar jcvf hello.tar.bz2 hello
```

* 解压缩bzip2

```
tar jxvf hello.tar.bz2
```

* 压缩为xz

```
tar Jcvf hello.tar.xz hello 
```

* 解压缩为xz

```
tar Jxvf hello.tar.xz
```

