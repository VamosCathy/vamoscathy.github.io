---
layout: post
title: vim使用笔记
comments: true
tags: vim
description: vim能让你敲代码敲得飞起，但前提是你必须对其使用比较熟悉。这篇文章就零碎地记录我在使用过程中遇到的命令，技巧，或者我认为比较神奇的东西。前期可能比较杂乱，在积累到一定量时进行整理。
categories: tech
---

## 一、快捷键

### 1.1 代码折叠／展开

在刚开始用zsh的时候，我就参考了池建强老师博客中的一篇vim文章，对vim进行了一定的配置，包括自动代码折叠。例如我写了如下一段代码：
{% highlight C %}
void reverse(char *s,int from,int to)
{
	while(from < to)
	{
		char t = s[from];
		s[from++] = s[to];
		s[to--] = t;
	}
}
{% endhighlight %}

在vim编辑器里会被如下折叠起来：
{% highlight C %}
4 --- 8 lines: {----------------}
{% endhighlight %}

在vim非编辑模式下，将光标停留在折叠行上，相应的折叠／展开的命令如下：

* zo 展开
* zc 收起
* zn 全部展开
* zN 全部折叠

### 1.2 插入
1. 字符位置插入
	* i 	在光标之前插入
	* a 	在光标之后插入
2. 行位置插入
	* A 	在一行的结尾处追加
	* I 	在一行的开头处插入
	* o 	在光标所在位置的下一行打开新行插入
	* O 	在光标所在位置的上一行打开新行插入