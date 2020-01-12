 队列同步器AbstractQueuedSynchronizer（以下简称同步器），是用来构建锁或者其他同步组件的基础框架。



AbstractQueuedSynchronizer：

1、CLH，FIFO 等待队列和维护一个 state 状态变量（原子更新）

- getState
- setState
- compareAndSetState

剩下就是排队和阻塞机制



2、支持独占（exclusive）和（shared）共享模式

独占：Mutex

两种模式都支持：ReadWriteLock

ConditionObject#isHeldExclusively：判断是独占还是共享？



3、核心方法

- tryAcquire
- tryRelease
- tryAcquireShared
- tryReleaseShared
- isHeldExclusively

重新定义上面这几个方法，通过 getState、setState 和 compareAndSetState 来检查 and/or 修改同步状态 state 的值，实现同步器。

这几个方法默认都是 throw UnsupportedOperationException，需要去实现的（按需）。

AbstractOwnableSynchronizer 可以通过这个接口定义的方法知道是谁在占有锁。



4、并不是完全按照 FIFO 的策略来的

它会先去尝试获取锁，如果获取失败，才会被加到队列里面去（第一次是抢占式，可能造成当前线程阻塞）



5、prev 主要用来处理取消（线程状态）的，next 实现阻塞语义。



6、条件队列？主队列？



7、只有成功 acquire ，节点才会变成 head

next，入队完成之后的下一步才设置 next，所以如果一个节点的 next 为 null，不一定是 tail

首节点是当前持有锁的线程？释放锁是会将下一个获得锁的节点设为头结点？



8、waitStatus

- static final int CANCELLED =  1; 表示当前节点的线程已经被取消了。什么时候取消的？如何取消的？
- static final int SIGNAL    = -1; 表示当前节点的后继节点线程需要被唤醒。
- static final int CONDITION = -2; 表示当前节点线程正在等待一个条件 （只会在条件队列中出现，CLH 队列不会出现）
- static final int PROPAGATE = -3; 表示 acquireShared 的时候需要无条件传播。？？？什么意思






如何管理同步状态的？

同步队列（一个FIFO双向队列）是AQS的核心，用来完成同步状态的管理，当线程获取同步状态失败时，AQS会将当前线程以及等待状态等信息构造成一个节点并加入到同步队列，同时会阻塞当前线程。



但是AQS已经把其返回值的语义定义好了：负值代表获取失败；0代表获取成功，但没有剩余资源；正数表示获取成功，还有剩余资源，其他线程还可以去获取。



```java
Node node = new Node(Thread.currentThread(), mode);

Node pred = tail;

if (pred != null) {
	node.prev = pred;
	if (casTail(node)) {
		pred.next = node; // next 为 null 的不一定是 tail 节点，这里体现出来了。先 casTail，然后在设置原来的 tail 的 next 为 node
		return node;
	}
}

enq(node);
return node;
```



addWaiter(Node mode) 将当前线程加到队尾，并指定获取资源的模式

setHead(Node node) 当获取到资源时，调用，将 node 设置为 head，从而出队？

unparkSuccessor(Node node) 唤醒 node 的后继节点（即 next），如果 next 为 null 或者被取消（waitStatus  > 0），就从队尾开始往前循环，找到第一个没有被取消的 successor。（为什么不直接从前往后找？）

doReleaseShared() 释放 shared 资源，给 successor 发信号并传播

如果节点取消，node.next = node;



shouldParkAfterFailedAcquire(Node pred, Node node) 获取资源失败，判断是否需要 park。

- 如果 pred == node.prev 且 pred.waitStatus == SIGNAL，返回 ture
- pred.waitStatus > 0，说明上一个节点取消了，将取消的节点全部剔除，返回 false，进入下一次循环。
- pred.waitStatus = 0 or PROPAGATE，尝试将设置前一个节点的 waitStatus = SIGNAL



```java
final boolean acquireQueued(final Node node, int arg) {
    boolean failed = true;
    try {
        boolean interrupted = false;
        for (;;) {
            final Node p = node.predecessor();
            if (p == head && tryAcquire(arg)) {
                setHead(node);
                p.next = null; // help GC
                failed = false;
                return interrupted;
            }
            if (shouldParkAfterFailedAcquire(p, node) &&
                parkAndCheckInterrupt())
                interrupted = true;
        }
    } finally {
        if (failed) // 这个地方到底有没有意义？是防止 tryAcquire 抛异常吗？只有这里出现异常，才会走这个代码，然后才会调用 cancelAccquire(node);
            cancelAcquire(node);
    }
}
```
acquireQueued(final Node node, int arg)  对已经在队列中的线程采用排他不可中断模式获取资源。返回在此期间是否中断过。 只有获取资源才会返回，否则一直挂起状态。



doAcquireInterruptibly(int arg) throws InterruptedException 排他可中断方式。在排队获取资源时如果发生中断，抛异常，直接返回。

doAcquireNanos(int arg, long nanosTimeout) throws InterruptedException  排他 + 最大等待时长模式。可中断。当获取失败时判断是否需要挂起，如果需要挂起，并且最大等待时长 > 系统默认的一个时长 1000 ns，则挂起线程，否则不会挂起当前线程（算是一个优化吧，挂起一个线程和让它在 1000 ns 自旋性能要好？）。



doAcquireShared(int arg) 获取共享模式，不可中断。它比排他模式多了一个操作。在获取资源时会返回一个值，这个值为负，代表获取失败；值为 0 代表获取成功但没有剩余的资源了；值为正代表获取成功且还有剩余的资源。当获取成功时，会把自己设置成  head 并 Propagate



doAcquireSharedInterruptibly(int arg) throws InterruptedException 获取共享模式，可中断。除了抛中断异常，其他和上面一样。



doAcquireSharedNanos(int arg, long nanosTimeout) throws InterruptedException 共享 + 最大等待时长模式。可中断。在上面的方法的基础上加了一个时间限制，如果超过了这个时间还没有获取到，返回 false。



排他模式获取资源和释放资源：

tryAcquire(int arg)

tryRelease(int arg)

共享模式获取资源和释放资源：

tryAcquireShared(int arg)

tryReleaseShared(int arg)



isHeldExclusively() 判断是否自己持有



acquire(int arg)

acquireInterruptibly(int arg) throws InterruptedException

tryAcquireNanos(int arg, long nanosTimeout) throws InterruptedException

release(int arg)

acquireShared(int arg)

acquireSharedInterruptibly(int arg) throws InterruptedException

tryAcquireSharedNanos(int arg, long nanosTimeout) throws InterruptedException

releaseShared(int arg)



hasQueuedThreads() 判断是否有排队的线程

hasContended() 判断是否发生过竞争



ConditionObject

addConditionWaiter() 将当前线程加入到条件等待队列中。尾插法。

doSignal(Node first) 将节点从条件等待队列中移至同步队列中。转换失败说明当前节点已被取消，则会找下一个没有被取消的节点，直到找到或者遍历完条件等待队列也没有（说明所有节点都被取消了。）

doSignalAll(Node first) 将条件队列的节点全部转换至同步队列中。

unlinkCancelledWaiters() 从头结点遍历，去掉所有已经取消的节点。

signal() 将等待时间最长的（第一个节点）从条件等待队列移至拥有锁的同步队列。

signalAll() 和上面一样，不过是将所有的节点

awaitUninterruptibly() 不可中断条件等待