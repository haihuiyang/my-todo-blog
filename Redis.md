> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



数据在内存里面，可做持久化存储。RDB 和 AOF 两种，一种是存数据的 snapshot 的形式（RDB），另一种是记录服务器执行的所有写操作的命令，并在服务器启动时，通过重新执行这些命令来还原数据集。



#### 基础

- 持久化的存储器 key-value

- key 和值，值可以是任何东西，Redis 才不会关心值，无法通过值进行查找

- 存储器内存储？（in-memory persistent store）什么是存储器？

- 查看 redis 各个命令的性能

  ```bash
  /Users/happyfeet/tools/redis/redis-5.0.7/src/redis-benchmark
  ```

- redis 提供了极好的性能，也因此付出了代价：使用场景有限



#### 5 种数据结构

数据结构是什么？有哪些方法？可用于哪些场景？

> flushdb 可以清除数据库中的所有值。

- 字符串（String）

set、get：

```bash
set key value [失效时间等参数]
get key
```

例如：

```bash
set age 24
get age
```



incr、incrby、decr、decrby：视频的点赞数、评论数、播放数统计。

```bash
> incr stats:page:about
(integer) 1
> incr stats:page:about
(integer) 2

> incrby ratings:video:12333 5
(integer) 5
> incrby ratings:video:12333 3
(integer) 8
```

getbit、setbit：“今天我们有多少个独立用户访问？” （[REDIS BITMAPS – FAST, EASY, REALTIME METRICS](https://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/)）：Web 应用中常见的问题。

getbit、setbit：`setbit key offset value` 

例：`setbit happyfeet 10 1` 

类似于这种存储，`0000001100000`，value 只能是 0 或 1（bit）。

这种 setbit 底层是如何存储的呢？如果 `setbit happyfeet 10086 1` 那就会使用一个长度为 10086 的数组来存吗？还是说存 bit 位，比如 01 存的值为 1，11 存的值为 3 这种？

- 哈希表（Hash）

hset、hget：

```bash
hset key field value
hget key field
```

例如：

```bash
hset happyfeet age 24
hget happyfeet age
```

还支持多个域的设置、多个域的获取、获取所有的域和值，列出所有的域或者删除指定的一个域：

```bash
hmset key field value [field value ...]
hmget key field [field ...]
hgetall key
hkeys key
hdel key field [field ...]
```

- 列表（List）

```bash
lpush key value [value ...]
lpop key
lrange key start stop
ltrim key start stop
```

- 集合（Set）

```bash
sadd key member [member ...]
sismember key member
sinter key [key ...]
sinterstore destination key [key ...]
smembers key
```

- 有序集合（SortedSet）（底层由 SkipList 实现）

```bash
zadd key [NX|XX] [CH] [INCR] score member [score member ...]
zcount key min max
zrank key member
zrevrank key member
```



#### 时间复杂度

Redis 对于每一个命令的时间复杂度都有解释

O(1)：sismember

O(log(N))：zadd

O(N)：ltrim

O(log(N)+M)：zremrangebyscore（`zremrangebyscore key min max`）

O(N+M*log(M))：sort



为什么 Redis 是单线程的？它是如何实现的？（网络请求模块使用了一个线程？）

为什么 Redis 这么快？

可不可以设置一个 key，但是类型不一样的情况？比如：set happyfeet "hello"；hset happyfeet age 24 这种



#### 流水线功能

```ruby
redis.pipelined do
  9001.times do
	redis.incr('powerlevel')
  end
end
```

#### 事务

每一个 Redis 命令都具有原子性。

```ruby
multi
...
exec
```

discard

Redis 是单线程运行的，但是可以同时运行多个 Redis 客户端进程，还是会出现并发问题。（如何理解？？？通过端口连接会不会有这种情况？）watch 解决？

- #### 关键字反模式（keys Anti-Pattern）



> 一些命令：info、select（切换指定数据库）、flushdb、multi、exec、discard、watch、keys



- 使用期限（Expire）

expire

expireat

ttl

persist

setex



- 发布和订阅（Pub/Sub）

blpop

brpop

从列表返回且删除第一个或最后一个元素，或者被阻塞，直到有元素可供操作。（类似于生产者-消费者模式）

subscribe

publish（返回值为数字，代表接收到消息的客户端数量）



- 监控与延时日志

monitor（生产环境中谨慎运行）

slowlog

- 排序（Sort）

sort：强大的排序命令



#### 管理

- 配置

redis.conf

config set

config get

- 验证

requirepass

auth password

- 大小限制
- 复制





##### 缓存雪崩：

- 大量缓存突然失效，例如：设置了失效时间的缓存 expire，集中在一起失效；
- 解决方案：随机失效时间

##### 缓存穿透：

- 缓存中和数据库中都没有的数据，
- 解决方案：布隆过滤器（利用高效的数据结构及算法判断这个 key是否在数据库里面）

##### 缓存击穿：

- 某一个热点 key 的请求特别多，当这个 key 失效时，持续的大并发穿破缓存，直接请求数据库

- 解决方案：热点数据永不过期、互斥锁



Redis 分布式锁（Redis 作为中间件 setnx+expire，SET IF NOT EXISTS）Lua 脚本优化：jedis.eval(lua_scripts, keys, values);



尾巴：



参考资料：

（1）

（2）