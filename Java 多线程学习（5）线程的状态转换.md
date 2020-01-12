线程的 6 种状态（Thread 类中定义了一个枚举 State ）：

NEW（新建）：创建后尚未启动的线程处于这种状态。

RUNNABLE（运行）：Runable 包括了操作系统线程状态中的 Running 和 Ready，也就是处于此状态的线程有可能正在执行，也有可能正在等待着 CPU 为它分配执行时间。

BLOCKED（阻塞）：线程被阻塞了，“阻塞状态” 与 “等待状态” 的区别是： “阻塞状态” 在等待着获取到一个排他锁，这个事件将在另外一个线程放弃这个锁的时候发生；而 “等待状态” 则是在等待一段时间，或者唤醒动作的发生。在程序进入同步区域的时候，线程将进入这种状态。

WAITING（无限期等待）：处于这种状态的线程不会被分配 CPU 执行时间，它们要等待被其他线程显式地唤醒。以下方法会让线程陷入无限期的等待状态：

- 没有设置 Timeout 参数的 Object.wait() 方法。
- 没有设置 Timeout 参数的 Thread.join() 方法。
- LockSupport.park() 方法。

TIMED_WAITING（限期等待）：处于这种状态的线程也不会被分配 CPU 执行时间，不过无须等待被其他线程显式地唤醒，在一定时间之后它们会由系统自动唤醒。以下方法会让线程进入限期等待状态：

- Thread.sleep() 方法。
- 设置了 Timeout 参数的 Object.wait() 方法。
- 设置了 Timeout 参数的 Thread.join() 方法。
- LockSupport.parkNanos() 方法。
- LockSupport.parkUntil() 方法。

TERMINATED（结束）：已终止线程的线程状态，线程已经结束执行。



yield()：当前线程让出 CPU，（给调度程序的提示是当前线程愿意放弃当前使用的处理器。）



线程的一些知识点：

创建线程的两种方式：

- 继承 Thread
- 实现 Runnable 接口，然后 new Thread(runnable).start();

线程有优先级



参考资料：

（1）Thread 源码

（2）《深入理解 Java 虚拟机》