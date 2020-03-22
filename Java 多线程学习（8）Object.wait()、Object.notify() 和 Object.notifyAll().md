> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)

这篇文章本来很早之前就准备写的，由于各种各样的原因一直没有开始动笔，一段时间不写博客，感觉都有点生疏了。

我们先来看看总共有哪几个方法吧：

>Object.wait()
>
>Object.wait(long timeout)
>
>Object.wait(long timeout, int nanos)
>
>Object.notify()
>
>Object.notifyAll()

一共这五个方法，都是 Object 对象提供的方法。

Object.wait(long timeout)

Object.notify()

Object.notifyAll()

这三个是 native 方法，剩下两个 wait 方法都是 Object.wait(long timeout)  的变形。



这几个方法都是 native 方法，平时用的好像也不多，写个例子直接调用 wait() 方法，发现竟然抛出了 IllegalMonitorStateException 异常。

看 Java 文档，知道他们的特性。看了源码实现后，他们就不再是黑盒子了



---

先描述这几个方法的基本用法

再从源码级别分析这几个方法为什么是这样的



入口

1、native 方法入口

```c++
static JNINativeMethod methods[] = {
    {"hashCode",    "()I",                    (void *)&JVM_IHashCode},
    {"wait",        "(J)V",                   (void *)&JVM_MonitorWait},
    {"notify",      "()V",                    (void *)&JVM_MonitorNotify},
    {"notifyAll",   "()V",                    (void *)&JVM_MonitorNotifyAll},
    {"clone",       "()Ljava/lang/Object;",   (void *)&JVM_Clone},
};
```

2、wait 方法

```c++
JVM_ENTRY(void, JVM_MonitorWait(JNIEnv* env, jobject handle, jlong ms))
  JVMWrapper("JVM_MonitorWait");
  Handle obj(THREAD, JNIHandles::resolve_non_null(handle));
  JavaThreadInObjectWaitState jtiows(thread, ms != 0);
  if (JvmtiExport::should_post_monitor_wait()) {
    JvmtiExport::post_monitor_wait((JavaThread *)THREAD, (oop)obj(), ms);

    // The current thread already owns the monitor and it has not yet
    // been added to the wait queue so the current thread cannot be
    // made the successor. This means that the JVMTI_EVENT_MONITOR_WAIT
    // event handler cannot accidentally consume an unpark() meant for
    // the ParkEvent associated with this ObjectMonitor.
  }
  ObjectSynchronizer::wait(obj, ms, CHECK);
JVM_END
```

3、



http://hg.openjdk.java.net/jdk8/jdk8/jdk/file/687fd7c7986d/src/share/native/java/lang/Object.c#l44



参考自己手写的草稿



park and unpark

pthread_cond_wait() 用于阻塞当前线程，等待别的线程使用pthread_cond_signal() 或 pthread_cond_broadcast来唤醒它。 pthread_cond_wait() 必须与pthread_mutex
配套使用。pthread_cond_wait()函数一进入wait状态就会自动release mutex。当其他线程通过pthread_cond_signal()或pthread_cond_broadcast，把该线程唤醒，使pthread_cond_wait()通过（返回）时，该线程又自动获得该mutex。











尾巴：

近段时间的事情还是蛮多的：从离职、面试，到换城市、租房子，差不多一个月的时间。虽然说平时的时间也不少，但毕竟需要重新适应新的节奏，所以博客也一直没有更新。

我只是业余的 C++ 爱好者，如果有写的不对的地方，欢迎大家指正！

参考资料：

（1）

（2）