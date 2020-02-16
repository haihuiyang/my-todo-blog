> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



需要写具体的例子来验证每个级别的具体情况。

事务传播机制：(Propagation)

- REQUIRED：默认级别，如果当前方法上层没有事务，则创建一个新的事务，当前方法在新的事务中执行；如果有事务，直接在原有事务中执行；

- REQUIRES_NEW：如果当前方法上层没有事务，则创建一个新的事务，当前方法在新的事务中执行；如果有事务，挂起原有事务，创建一个新的事务，当前方法在新事务中执行，待当前方法执行完成后，唤起原有事务；

- MANDATORY：如果当前方法上层没有事务，会抛出 TransactionRequiredException 异常；如果有事务，在原有事务中执行；

- SUPPORTS：如果当前方法上层没有事务，当前方法以非事务的形式执行；如果有，在原有事务中执行；

- NOT_SUPPORTED：如果当前方法上层没有事务，当前方法以非事务的形式执行；如果有，挂起原有事务，当前方法以非事务的形式执行，执行完成后，唤起原有事务；

- NEVER：如果当前方法上层没有事务，当前方法以非事务的形式执行；如果有，会抛出 InvalidTransactionException 异常
- NESTED：据说和 REQUIRES_NEW 的区别在于它和父事务是相依的，是一起提交的，而 REQUIRES_NEW 是相互独立的

 可以从是否新建事务、是否在事务中执行，是否允许在事务中执行这几个方面来总结。



隔离级别：（Isolation）

- DEFAULT：各个数据库的默认隔离级别；比如：MySQL InnoDB 引擎下是 REPEATABLE_READ 隔离级别
- READ_UNCOMMITTED：未提交读
- READ_COMMITTED：提交读
- REPEATABLE_READ：可重复读
- SERIALIZABLE：可串行化



尾巴：



参考资料：

（1）

（2）