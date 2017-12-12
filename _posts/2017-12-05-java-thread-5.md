---
layout: post
title:  "Java 多线程学习(五)：synchronized"
date: 2017-12-5 17:47:16
tags: 
    - java
    - Java Thread 
    - Concurrence
categories: Java 多线程学习 
---

{:toc}

在前面的文章中，我们经常提到`synchronized`关键字，它的字面意思是：同步的。而在java中，`synchronized`关键字作为一个重要的同步工具之一，它能告诉JVM**某个代码块**或者**某个方法**需要做同步处理。

<!--more-->
## 基本原理


## 如何使用
在java中`synchronized`可以有三种使用方式:  
> 1. 同步代码块  
> 2. 同步方法  
> 3. 静态同步方法

### 同步代码块
同步代码块的意思就是将需要同步的代码放到代码块中，并且**指定需要锁住的非空对象**。

例如：
{% highlight java linenos%}
public class SimpleCounter {
    private int count;
    public void increase(){
        count++;
    }
    public static void main(String[] args) throws InterruptedException {
        SimpleCounter counter = new SimpleCounter();
        Runnable runnable = () -> {
			synchronized (counter){
                counter.increase();
                /**
                 * 不使用synchronized的结果为 2 3 4 2 5 6 7 8 9 10	
                 * 使用synchronized (counter)结果为1 2 3 4 5 6 7 8 9 10
                 */
                System.out.print(counter.count + "\t");
			}
        };
        for (int i = 0; i < 10; i++) {
            new Thread(runnable).start();
        }
        Thread.sleep(100);
    }
}
{% endhighlight %}

### 同步方法
同步方法的意思就是将整个方法锁住，而加锁的对象是参数里配置的对象。如果没有配置的话默认是**this**，即当前对象。

把上面的例子改造一下：
{% highlight java linenos%}
public class SimpleCounter {
    private int count;
    public synchronized void increase(){
        count++;
        /**
         * 不使用synchronized的结果为 2 3 4 2 5 6 7 8 9 10	
         * 使用synchronized (counter)结果为1 2 3 4 5 6 7 8 9 10
         */
        System.out.print(count + "\t");
    }
    public static void main(String[] args) throws InterruptedException {
        SimpleCounter counter = new SimpleCounter();
        Runnable runnable = () -> counter.increase();
        for (int i = 0; i < 10; i++) {
            new Thread(runnable).start();
        }
        Thread.sleep(100);
    }
}
{% endhighlight %}

ps.这里需要注意的是，在例子中我们把`System.out.print()`方法也放到了`increase()`方法中。这是因为，`count + "\t"`这句代码并不是同步的。

### 静态同步方法
我们都知道静态方法并不属于任何一个具体的类。所以，如果我们需要创建一个同步的静态方法的话，加锁的就不再是对象而是这个Class。

同样适用上面的例子改造一下：
{% highlight java linenos%}
public class SimpleCounter {
    public static int count;
    public synchronized static void increase() throws InterruptedException {
        count++;
		/**
         * 不使用synchronized的结果为 2 4 3 2 6 5 7 8 9 10		
         * 使用synchronized (counter)结果为1 2 3 4 5 6 7 8 9 10
         */
        System.out.print(count + "\t");
        Thread.sleep(10);
    }
    public static void main(String[] args) throws InterruptedException {
        Runnable runnable = () -> {
            try {
                SimpleCounter.increase();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        };
        for (int i = 0; i < 10; i++) {
            new Thread(runnable).start();
        }
        Thread.sleep(100);
    }
}
{% endhighlight %}

## 结束

>**本文部分文本来源于互联网**

>**感谢以下文章提供的灵感和帮助**  
> - [Java并发编程的总结与思考](http://www.jianshu.com/p/053943a425c3)  