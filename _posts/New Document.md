## JVM中的线程简介 
在JVM中，每一次启动就会创建一堆的线程，为了分析JVM启动了哪些线程，我们需要`JAVA Thread Dump`这个工具获取JVM的线程堆栈信息。
第一步，一个简单的java应用。为了方便获取堆栈信息，我使用控制台输入的方式防止线程直接终止：
{% highlight java %}
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(scanner.nextLine() + " hello!");
    }
}
{% endhighlight %}
第二步，运行java应用，并使用JDK自带的jstack工具导出线程堆栈信息：
{% highlight cmd %}
jstack [-l ] <进程pid> > jstack.log
{% endhighlight %}
获取的堆栈信息如下：
{% highlight java %}
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.73-b02 mixed mode):

"Service Thread" #10 daemon prio=9 os_prio=0 tid=0x000000001e18b800 nid=0x3838 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C1 CompilerThread3" #9 daemon prio=9 os_prio=2 tid=0x000000001e102800 nid=0xb70 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread2" #8 daemon prio=9 os_prio=2 tid=0x000000001e0fd800 nid=0x30a0 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread1" #7 daemon prio=9 os_prio=2 tid=0x000000001e0f6800 nid=0xb2c waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" #6 daemon prio=9 os_prio=2 tid=0x000000001e0d6000 nid=0x3c10 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Attach Listener" #5 daemon prio=5 os_prio=2 tid=0x000000001e0d5000 nid=0x4198 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Signal Dispatcher" #4 daemon prio=9 os_prio=2 tid=0x000000001e0d3800 nid=0x35a4 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Finalizer" #3 daemon prio=8 os_prio=1 tid=0x000000001ceff800 nid=0x2550 in Object.wait() [0x000000001f59f000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x000000076b907110> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:143)
	- locked <0x000000076b907110> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:164)
	at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:209)

"Reference Handler" #2 daemon prio=10 os_prio=2 tid=0x000000001cef9000 nid=0x42bc in Object.wait() [0x000000001f45f000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x000000076b906b50> (a java.lang.ref.Reference$Lock)
	at java.lang.Object.wait(Object.java:502)
	at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:157)
	- locked <0x000000076b906b50> (a java.lang.ref.Reference$Lock)

"main" #1 prio=5 os_prio=0 tid=0x000000000268f800 nid=0x43cc runnable [0x00000000032de000]
   java.lang.Thread.State: RUNNABLE
	at java.io.FileInputStream.readBytes(Native Method)
	at java.io.FileInputStream.read(FileInputStream.java:255)
	at java.io.BufferedInputStream.read1(BufferedInputStream.java:284)
	at java.io.BufferedInputStream.read(BufferedInputStream.java:345)
	- locked <0x000000076b959e58> (a java.io.BufferedInputStream)
	at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:284)
	at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:326)
	at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:178)
	- locked <0x000000076b9e5e60> (a java.io.InputStreamReader)
	at java.io.InputStreamReader.read(InputStreamReader.java:184)
	at java.io.Reader.read(Reader.java:100)
	at java.util.Scanner.readInput(Scanner.java:804)
	at java.util.Scanner.findWithinHorizon(Scanner.java:1685)
	at java.util.Scanner.nextLine(Scanner.java:1538)
	at Main.main(Main.java:7)

"VM Thread" os_prio=2 tid=0x000000001e072000 nid=0x43d4 runnable 

"GC task thread#0 (ParallelGC)" os_prio=0 tid=0x000000000255d000 nid=0x29b0 runnable 

"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x000000000255e800 nid=0x21fc runnable 

"GC task thread#2 (ParallelGC)" os_prio=0 tid=0x0000000002560000 nid=0x3af0 runnable 

"GC task thread#3 (ParallelGC)" os_prio=0 tid=0x0000000002561800 nid=0x4114 runnable 

"GC task thread#4 (ParallelGC)" os_prio=0 tid=0x0000000002564800 nid=0x3f38 runnable 

"GC task thread#5 (ParallelGC)" os_prio=0 tid=0x0000000002566000 nid=0x3c24 runnable 

"GC task thread#6 (ParallelGC)" os_prio=0 tid=0x0000000002569000 nid=0x3d44 runnable 

"GC task thread#7 (ParallelGC)" os_prio=0 tid=0x000000000256a000 nid=0x28d0 runnable 

"VM Periodic Task Thread" os_prio=2 tid=0x000000001e14c800 nid=0x2b1c waiting on condition 

JNI global references: 6
{% endhighlight %}

>ps.为了方便阅读，我将线程中的`Locked ownable synchronizers: - None `去掉了，这一段的意思很神秘，我在stackoverflow的答 案-[What is “Locked ownable synchronizers” in thread dump?](https://stackoverflow.com/questions/41300520/what-is-locked-ownable-synchronizers-in-thread-dump)中看了一下，依然不是很明白。如果有朋友了解这一块的话，可以在留言中告诉我一下，谢谢。

### 线程类型
JVM的线程类型分为以下两种：  
1. daemon threads  
2. non-daemon threads  
**Daemon threads**是JVM默认创建的系统线程，用于处理内存回收任务或者`JMX`;  
**non-daemon threads**也就是java应用创建的非JVM系统线程，当所有的`non-daemon threads`停止工作的时候，`Daemon threads`也会停止工作。