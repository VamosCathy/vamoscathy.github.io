---
layout: post
title: 编程学习——字符串包含
categories: ProgrammingStudy
tags: 
 - string
comments: true
modified: 2015-6-9
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

与计数比较法相似的方法是利用**hashtable**，代码如下：

{% highlight C++ %}
bool stringContain_hashtable(string &a,string &b){
	vector<int> hash;
	hash.resize(26,0);
	int m = 0; //m可计算出现的不同字母个数
	for(int i = 0; i < b.length(); ++i){ //b串为短字符串
		int x = b[i] - 'A';
		if(hash[x] == 0){
			hash[x] = 1;
			m++;
		}
	}
	for(int i = 0; i < a.length() && m > 0; ++i){
		int x = b[i] - 'A';
		if(hash[x] == 1)
			hash[x] = 0;
			--m; //重复不计
	}
	return m == 0;
}
{% endhighlight %}

一种是**素数相乘法**，很好理解，就是把每个字母映射为一个素数，然后将长字符串中的字母代表的素数相乘，再将短串代表的素数去除这个数。如果有余数则返回false，否则返回true。但是这种方法有个显然的问题，数字相乘容易产生溢出风险，字符串稍长就不具有实际意义了。

最好的思路是**位运算**，该方法的时间复杂度为$$O(m+n)$$，空间复杂度为$$O(1)$$。这种方法用26bit表示长字符串的一个“签名”，这样就能以一个整数代表hashtable。代码如下：

{% highlight C++ %}
bool stringContain_bit(string &a,string &b){
/*位运算*/
    int hash = 0;
    for (int i = 0; i < a.length();++i)
        hash |= (1 << (a[i] - 'A'));
    for (int i = 0; i < b.length();++i){
        if((hash & (1 << (b[i] - 'A')) == 0)
            return false;
    }
    return true;
}
{% endhighlight %}

## 总结

综上，对于字符串包含的问题，最好的思路就是使用位运算，其本质就是使用一个整数作为hashtable的变形，这样时间复杂度和空间复杂度都最小，且不需要排序。
