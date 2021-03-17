> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---

背景：

MySQL 插入一条中文报错：

```mysql
Incorrect string value: '\xE5\xBC\xA0\xE4\xB8\x89' for column 'ColumnName' at row 1
```



https://www.cnblogs.com/yangmingxianshen/p/7999428.html

查看字符集：

```mysql
show variables like '%character%';
```

```mysql
show database status from 库名 like  表名;
```

```mysql
show table status from 库名 like  表名;
```

修改字符集：





尾巴：



参考资料：

（1）资料 A

（2）资料 B