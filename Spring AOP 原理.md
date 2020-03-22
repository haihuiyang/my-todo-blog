> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



Aspect Oriented Program 面向切面编程

@Aspect 注解可以实现；平时用的最多的还是 @Transactional 注解



```java
beforeInvokeMethod();

invokeMethod();

afterInvokeMethod();	
```



**对很多功能都有的重复代码的抽取，在运行时往业务方法上动态注入 “切面类代码”。** 

**让关注点代码和核心业务代码分离！**





尾巴：



参考资料：

（1）

（2）