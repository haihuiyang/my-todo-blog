> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



DispatcherServlet（请求过来）

HanlderMapping（根据 URL 找到具体的 Controller）

Controller（处理，封装返回 ModelAndView）

ModelAndView

ViewResolver（视图解析器）



参考源码写一篇博客



1、前端发生请求，dispatcher 收到，丢给 handerMapping

2、handerMapping 根据 URL 找到具体的 Controller

3、Controller 处理，返回 ModelAndView

4、ViewResolver 解析视图，返回给前端



尾巴：



参考资料：

（1）

（2）