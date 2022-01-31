---
title: linux常用命令
date: 2022-01-13 20:30:18
tags: linux
categories: skills
---



持续更新

<!--more-->

* cd
* cd ..
* pwd 显示目录路径
* ls(ll)
  *  ls -lh 显示目录下文件的详细属性
  *  ls -a 显示隐藏文件
  *  ls -i 在第一列显示inode号
  *  ls -t 根据修改时间排列
  *  ls -s 以块数表示每个文件分配的尺寸
  *  ls -S 以文件大小排序
* touch 新建一个文件
* rm 删除一个文件
  * rm -r 删除目录内部的所有内容
  * rm -f 强制删除
* mkdir 新建一个目录
  * mkdir -m 设置权限，默认为755
  * mkdir -v 显示信息
* rmdir 删除空目录
* cp 复制文件和目录 cp 文件路径 目录路径
* rm -r 删除一个文件夹
* mv 移动文件 
* reset 重新初始化终端
* clear
* history
* help
* exit 退出
* #表示注释
* wc 统计文件行数、单词数、字节数和字符数。
  * wc -l 统计行数
  * wc -w 统计单词数
  * wc -c 统计字节数
  * wc -m 统计字符数
  * wc -L 最长行的长度

* echo 输出命令
  * echo hello > /root/a   新建a文件，其中写入hello
  * echo Linux >>  /root/a 在a中后面写入Linux

* ln 建立链接
  * ln -s 建立软链接
  * ln 建立硬链接

* cat 显示文本文件
  * cat -n  显示行号
  * cat -s 合并空白行
  * cat -b 非空输出行编号
  * cat -E 每行结束显示`$`
* sort 文件 排序
* chmod 设置权限
  * chmod u+x 设置可执行权限
