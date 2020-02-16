> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



浏览器  <==> 服务器

HTTP

接口协议

当浏览器发出一个GET请求时，就意味着要么是用户自己在浏览器的地址栏输入，要不就是点击了html里a标签的href中的url。所以其实并不是GET只能用url，而是浏览器直接发出的GET只能由一个url触发。所以没办法，GET上要在url之外带一些参数就只能依靠url上附带querystring。但是HTTP协议本身并没有这个限制。



浏览器中的 GET 和 POST：

- GET

读取一个资源

幂等（PUT、DELETE 也是幂等的），可保存为书签

对于 HTTP 协议来说，URL 长度没有限制，但对于一些浏览器来说有限制，浏览器对于 path 有长度限制。HTTP 协议本身是没有限制的。

GET 的安全性，路径可以直接拿到，某些日志记录的也是 path，如果放在路径上，很容易被记录下来；不过 POST 的 path 和 body 实际上也是明文传递的，其实也是不安全的，相比而言，POST 要安全一些。安全的做法应该是使用 HTTPS

一般情况下，私密数据传输用POST + body就好。

- POST

提交一个表单

不幂等，不可保存为书签



API 中的 GET 和 POST：

GET 照样可以使用 body 传数据，HTTP 协议并没有约束



POST 可支持 json、文件等格式



规定是这样规定的，但是实现是自己实现的，可以将 GET 实现成有副作用，比如：更新某个值；也可以将 POST 实现成无副作用的。



尾巴：



参考资料：

（1）

（2）