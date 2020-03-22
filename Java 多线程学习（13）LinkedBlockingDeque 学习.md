> 转载请注明原创出处，谢谢！
>
> [HappyFeet的博客](https://blog.csdn.net/haihui_yang)





---



LinkedBlockingDeque 中 137-149

```java
/**
 * Pointer to first node.
 * Invariant: (first == null && last == null) ||
 *            (first.prev == null && first.item != null)
 */
transient Node<E> first;

/**
 * Pointer to last node.
 * Invariant: (first == null && last == null) ||
 *            (last.next == null && last.item != null)
 */
transient Node<E> last;
```

这里

`(first == null && last == null) || (first.prev == null && first.item != null)`

和 

`(first == null && last == null) || (last.next == null && last.item != null)`

的意思是：

- 双向链表为空：first 和 last 两个节点为 null
- 双向链表不为空：first 的 prev 节点为 null，last 的 next 节点为 null



```java
/** Main lock guarding all access */
final ReentrantLock lock = new ReentrantLock();

/** Condition for waiting takes */
private final Condition notEmpty = lock.newCondition();

/** Condition for waiting puts */
private final Condition notFull = lock.newCondition();
```

ReentrantLock 的 notEmpty 和 notFull 两个 Condition 实现生产者-消费者等待



尾巴：



参考资料：

（1）资料 A

（2）资料 B