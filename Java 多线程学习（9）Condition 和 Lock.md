> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)

说来惭愧，又挺久没更新博客了。



---



Condition：wait set

Lock：monitor

多个 wait set，控制 notify 的顺序

同一个锁上多个 wait set，这是 object monitor 所做不到的

Lock，提供了比 synchronized 更多的使用方式

synchronized 的使用限制，催生了 Lock 和 Condition 的出现。

做了一层封装。



synchronized(lock1) {



​	doSomething



​	synchronized(lock2) {



​	}



}



如果一个线程获取了多个锁，比如上面的代码：依次获得了 lock1 和 lock2，synchronized 只能先释放 lock2，然后在释放 lock1，以和获取锁相反的顺序释放锁。

没办法做到先释放 lock1，然后再释放 lock2。



为了提供更灵活的方式，所以不再提供自动释放锁的操作，需要手动释放锁。一般结构如下：

```java
Lock l = …;
l.lock();

try {
	// access the resource protected by this lock
} finally {
	l.unlock();
}
```



在使用synchronized关键字获取锁的过程中不响应中断请求，这是synchronized的局限性。如果这对程序是一个问题，应该使用显式锁，java中的Lock接口，它支持以响应中断的方式获取锁。对于Lock.lock()，可以改用Lock.lockInterruptibly()，可被中断的加锁操作，它可以抛出中断异常。等同于等待时间无限长的Lock.tryLock(long time, TimeUnit unit)。



Thread.interrupt()：只是给线程设置中断标志位，告诉当前线程有被中断过，由线程自己决定是否需要中断；

wait、join、sleep 这种在 c++实现中会处理中断，如果在等待或睡眠过程中发生了中断，程序会抛出InterruptedException异常



尾巴：



参考资料：

（1）

（2）