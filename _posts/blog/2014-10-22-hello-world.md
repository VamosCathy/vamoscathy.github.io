---
layout: post
title: Powershell + Git，让windows上的编程更舒适
category: blog
description: Powershell为windows下的编程提供了如同LInux终端般的命令行输入窗口，而git则让代码管理更加清晰便捷。
---

一个星期在自己的windows系统上搭建了比较顺手的编程环境，并且借用了别人的样式创建了一个基于GitHub Pages的个人博客。一切都还属于起步阶段，这篇文章先记录一下基本的环境搭建。

## 复制Linux下的操作体验——Windows Powershell

直到下载了Github的windows下图形界面才知道windows已经自带了Powershell这样一个强大存在。关于它的基本介绍可以查看[PowerShell-wiki][1]，简单来说，在这样一个命令行界面里面，你可以愉快地在Windows下使用Linux命令，而且界面要比cmd好看多倍，就像下面这样：

![alt text](/images/Windows_PowerShell_1.0_PD.png "PowerShell-wiki")

![alt text](/images/powershell2.jpg "PowerShell-v4.0")

另外，到后面安装了Git之后，顺带也会把ssh给安装了，是不是有了一种都不需要用PUTTY的感觉了呢？

##让你的PowerShell用上Git

使用GitHub无疑是程序员的标志，它不仅仅是一个高效管理代码的工具，更是强大的编程学习和交流的工具。相对Linux和Mac OS而言，在Windows下使用Git要略麻烦一些，但是如果配合使用PowerShell，用起来还是很顺畅的。下面具体介绍安装配置方法：

###第一步，安装Git

{{page.date | date_to_string}}