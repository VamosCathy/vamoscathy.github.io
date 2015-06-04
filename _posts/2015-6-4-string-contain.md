---
layout: post
title: 编程学习——字符串包含
categories: ProgrammingStudy
tags: 
 - string
comments: true
description: 这段时间我通过阅读《The Art of Programming By July》来练习编程。这一篇是有关字符串包含问题的学习笔记。
---

*版权声明：本文为我在学习《The Art of Programming By July》的笔记，其中的题目和例题的代码均来自于书中，因此版权为书作者和为该书提供代码的编写者所有*

## 题目描述
> “给定两个分别由字母组成的字符串A和字符串B，字符串B的长度较字符串A短。请问，如何最快地判断字符串B中所有字母是否都在字符串A里？也就是说判断字符串B是不是字符串A的真子集（为了简化，姑且认为两个集合都不是空集，即字符串都不为空）。且为了简单起见，我们规定输入的字符串只包含大写英文字母。”
> 
> 摘录来自: July. “The Art of Programming By July”。 iBooks. 

## 题目解法

很显然，最暴力的方式就是暴力轮询，稍微想一下能想到的方法就是排序后再遍历（在允许排序的前提下）。

我们可以对排序法再进一步，用线性时间的**计数比较法**，时间复杂度为$$O(m+n)$$，代码如下：

{% highlight C++ %}
bool stringContain_count(string &a,string &b){
    vector<int> have;
    have.resize(26,0);
    for(int i = 0;i < a.length();i++)
        ++have[a[i] - 'A'];
    for(int i = 0;i < b.length();i++){
        if(have[b[i] - 'A'] == 0)
            return false;
    }
    return true;
}
{% endhighlight %}

在第二次对短字符串遍历时能以$$O(1)$$时间找到每个字母，所以总共只需要线性时间。