查看 jdk 源码笔记



1、获取 openjdk 源码

[openjdk 8 源码下载](http://jdk.java.net/java-se-ri/8)

（这里以 Java 8 为例，如需其他版本，可以在界面的左边选择对应的版本下载）

RI Source Code：[zip file](https://download.java.net/openjdk/jdk8u40/ri/openjdk-8u40-src-b25-10_feb_2015.zip)

```bash
mv ~/Downloads/openjdk-8u40-src-b25-10_feb_2015.zip ~/projects
unzip /Users/happyfeet/projects openjdk-8u40-src-b25-10_feb_2015.zip
```

通过 Sublime Text 3 打开，目录结构如下：

![openjdk 目录结构](https://tva1.sinaimg.cn/large/006tNbRwgy1gb0n2in878j30hm0tedgu.jpg)



2、查看顺序：

- 先找到对应 Java 类的 .c 文件，例如：String.c；（目录为：）
- 在 jvm.cpp 里面找具体的实现
- 查看实现，进入具体的实现（如果有疑问，Google，差不多能找到一些线索）

这样过一遍 C++ 代码，基本上大致的原理也就能知道了。



有一些例外，比如：Unsafe ，它位置是在 `openjdk/jdk/src/share/classes/sun/misc/Unsafe.java` 







参考链接：

1、[阅读openjdk源代码](https://hllvm-group.iteye.com/group/topic/35385)