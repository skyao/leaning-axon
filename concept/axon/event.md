# Event

这个 Event 指 DDD 中的 Domain Event，按照 DDD 的设计，Aggregate 在状态变更时需要发送 Domain Event 来通知系统中的其他组件（尤其是 Aggregate 自己也不知道到底哪些组件对自己的这个状态变更感兴趣）。

下面这样代码是刚才在 Aggregate 中看到的发送 Domain Event 的代码：

```java
public void deposit(long amount) {
    apply(new MoneyDepositedEvent(id, amount));
}

public void withdraw(long amount) {
    if (amount <= balanceInCents + overdraftLimit) {
        apply(new MoneyWithdrawnEvent(id, amount));
    }
}
```

这里的 apply() 方法最终会调用 eventBus 的 publish() 方法将 Domain Event 发送到 Event Bus：

```java
eventBus.publish(msg);
```

> 注： 具体的代码细节看后面章节的代码实现分析。
