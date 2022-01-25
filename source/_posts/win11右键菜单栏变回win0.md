---
title: win11右键菜单栏变回win0
date: 2022-01-24 21:45:45
tags: 
- win11
- win10
- 右键菜单
categories: 随笔
---

win11右键菜单栏变为win10版本的方法。

<!--more-->

新建一个文本文档

输入以下内容：

```bash
@echo off

:start

cls

echo,

echo 修改右键菜单模式

echo,

echo 1 穿越到Windows 10默认模式

echo,

echo 2 恢复为Windows 11默认模式

echo,

echo 0 什么也不做，退出

echo,

echo,

choice /c:120 /n /m:"请选择要进行的操作（1/2/0）："

if %errorlevel%==0 exit

if %errorlevel%==2 goto cmd2

if %errorlevel%==1 goto cmd1

exit

:cmd1

reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

taskkill /f /im explorer.exe

start explorer.exe

exit

:cmd2

reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f

taskkill /f /im explorer.exe

start explorer.exe

exit
```

另存为

![image-20220124214409716](win11%E5%8F%B3%E9%94%AE%E8%8F%9C%E5%8D%95%E6%A0%8F%E5%8F%98%E5%9B%9Ewin0/image-20220124214409716.png)

然后生成bat文件，直接右键-> 以管理员身份运行 即可。

