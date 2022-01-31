---
title: 找不到gpedit.msc怎么办
date: 2022-01-30 22:39:49
tags: 
- windows10
categories:
- skills
---

如果cmd中找不到gpedit.msc

新建一个文本文档输入

```bash
@echo off
pushd "%~dp0"
dir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~3*.mum >List.txt
dir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum >>List.txt
for /f %%i in ('findstr /i . List.txt 2^>nul') do dism /online /norestart /add-package:"C:\Windows\servicing\Packages\%%i"
pause
```

<!--more-->

改扩展名为cmd，管理员方式运行即可。

