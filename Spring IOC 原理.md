> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



一般来说，一个 ServiceA 一般会依赖多个其他的 Service，如果是自己创建、管理这些依赖的 Service，会很麻烦（你要知道这些 Bean 是怎么创建的，可能还会依赖其他的 Bean）；于是 Spring 就有了一个容器的概念，将创建 Service 的 Bean 交给 Spring 管理，ServiceA 的依赖关系也告诉 Spring，Spring 来将依赖的 Service 给到 ServiceA，这个思想就是 IOC，将控制权交给 Spring，而不是自己管理。







尾巴：



参考资料：

（1）

（2）

