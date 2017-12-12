---
layout: post
title:  "算法从头学(1) - 查找算法"
date: 2017-12-3 11:43:55
tags: 
    - java
    - Algorithm
    - Search algorithm 
categories: 算法从头学 
---

{:toc}

<blockquote class="blockquote-center">查找 -- 作为现代系统中最频繁的一个数据操作，查找功能的快速和稳定是最重要的。而查找算法作为查找功能的核心，需要根据不同系统的运行环境和实际要求进行慎重选择。</blockquote>

常见的排序算法有如下几种：

	1. 线性查找算法
	2. 二分查找算法
	3. 

## 1.线性查找算法
作为最常见、最简单的查找算法，它实现的方式也是简单粗暴：
> 一个一个挨着找。找到就返回结果，找不到继续找。如果直到最后一个元素都不匹配的话，就会返回没有找到匹配的元素。

我们举个简单的例子，下面的代码是ArrayList的`indexOf()`方法。
{% highlight java linenos%}
public int indexOf(Object o) {
    if (o == null) {
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
{% endhighlight %}

## 2.二分查找算法
**二分查找算法**，也称之为**折半搜索算法**、**对数搜索算法**。它的基本原理是：
>在特定的 **有序** 数组中，从数组的中间元素开始。如果中间元素正好是要查找的元素，则搜索过程结束；如果某一特定元素大于或者小于中间元素，则在数组大于或小于中间元素的那一半中查找，而且跟开始一样从中间元素开始比较。如果在某一步骤数组为空，则代表找不到。

具体过程如下图：
![](https://s1.ax1x.com/2017/12/08/7PSXQ.png)
(Binary_search_into_array From wikipedia)





## 结束

>**本文部分文本来源于互联网**

>**感谢以下文章提供的灵感和帮助**  
> - [Data Structure and Algorithms Linear Search](https://www.tutorialspoint.com/data_structures_algorithms/linear_search_algorithm.htm)  
> - [[Data Structure & Algorithm] 七大查找算法](http://www.cnblogs.com/maybe2030/p/4715035.html)