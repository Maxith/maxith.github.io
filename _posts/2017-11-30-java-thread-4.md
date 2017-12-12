---
layout: post
title:  "Java 多线程学习(四)：锁"
date: 2017-11-30 16:44:52
tags: 
    - java
    - Java Thread 
    - Lock
categories: Java 多线程学习 
---

{:toc}

在上一篇博文中，我们了解了线程的并发虽然能带来任务执行效率上的提升。但是，同时会带来很多麻烦，例如线程的相互干扰和共享对象操作结果的一致性错误。

为了解决线程并发而导致的错误，我们引入了**同步工具**，它能使多线程按照我们需要的顺序执行，防止产生线程干扰以及对象一致性错误等问题。

> 需要主要的是：同步工具**不当使用**会造成诸如线程运行缓慢，甚至暂停以致于发生死锁等严重后果。

<!--more-->




## 结束

>**本文部分文本来源于互联网**

>**感谢以下文章提供的灵感和帮助**  
> - [Java并发编程的总结与思考](http://www.jianshu.com/p/053943a425c3)  
> - [Java™教程 - 同步](https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html)