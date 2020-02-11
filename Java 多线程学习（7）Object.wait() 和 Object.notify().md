> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)

这几个方法都是 native 方法，看 Java 文档，知道他们的特性。看了源码实现后，他们就不再是黑盒子了



---



参考自己手写的草稿



park and unpark

pthread_cond_wait() 用于阻塞当前线程，等待别的线程使用pthread_cond_signal()或pthread_cond_broadcast来唤醒它。 pthread_cond_wait() 必须与pthread_mutex
配套使用。pthread_cond_wait()函数一进入wait状态就会自动release mutex。当其他线程通过pthread_cond_signal()或pthread_cond_broadcast，把该线程唤醒，使pthread_cond_wait()通过（返回）时，该线程又自动获得该mutex。











尾巴：

我只是业余的 C++ 爱好者，如果有写的不对的地方，欢迎大家指正！

参考资料：

（1）

（2）