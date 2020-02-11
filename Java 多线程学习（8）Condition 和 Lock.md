> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





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







尾巴：



参考资料：

（1）

（2）