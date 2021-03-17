> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)



---

tomcat 下载：https://tomcat.apache.org/download-70.cgi

web.xml 配置相关：

- https://segmentfault.com/a/1190000011404088

- https://blog.csdn.net/u010796790/article/details/52098258



所需依赖：

```java
<packaging>war</packaging>

    <properties>
        <spring.version>5.2.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>

    </dependencies>
```



关于 Java Web 的一些理解（杂文）：https://my.oschina.net/aaron74/blog/282304



web 项目结构

![web 项目结构](https://tva1.sinaimg.cn/large/007S8ZIlgy1genfh29gpbj30jl0evmxe.jpg)



尾巴：



参考资料：

（1）资料 A

（2）资料 B