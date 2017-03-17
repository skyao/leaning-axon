# Aggregate


前面介绍 CommandHandler 时，我们已经看过用来处理 DepositMoneyCommand 命令的代码：

```java
@CommandHandler
public void handle(DepositMoneyCommand command) {
	// 从 repository 中装载 BankAccount 的Aggregate
    Aggregate<BankAccount> bankAccountAggregate = repository.load(command.getBankAccountId());
    // 通过 bankAccountAggregate 来执行 deposit 操作
    // deposit 操作的实现是通过 BankAccount 这个 Aggregate 来执行的
    bankAccountAggregate.execute(bankAccount -> bankAccount.deposit(command.getAmountOfMoney()));
}
```

在 DDD 的设计中，这是 Aggregate 的标准使用方式：

1. Aggregate 只能通过 repository 装载
2. repository 通常喜欢根据 id 属性来装载 Aggregate
3. Aggregate 不是简单的VO，而是领域对象，不仅仅保持自身的数据和状态，而且有业务方法来实现业务逻辑

关于第三点，来看 BankAccount 的代码实现：

```java
// @Aggregate 注解表明这是一个 DDD 中的 Aggregate
@Aggregate
public class BankAccount {

	// @AggregateIdentifier 注解表明这个字段是 Aggregate 的 id 字段
    @AggregateIdentifier
    private String id;
    // Aggregate 是领域对象，保存有数据和状态
    private long overdraftLimit;
    private long balanceInCents;
	......
}
```

除此之外，BankAccount 这个 Aggregate 还定义有业务方法来对应各种业务错误，比如 deposit 和 withdraw：

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

和一般业务处理方式不同的是，axon 中的 Aggregate 不能直接在业务方法中简单的操作自身的状态，即不能直接通过 `this.balanceInCents=***;` 这种方式来修改状态。而是要发送一个 Domain Event 给 Event Bus，然后自己再定义对应的 Event Handler 来处理这个 Domain Event：

> 注： MoneyAddedEvent 是 MoneyDepositedEvent 和 MoneyWithdrawnEvent 的父类。

```java
@EventHandler
public void on(MoneyAddedEvent event) {
    balanceInCents += event.getAmount();
}
```

在绕了一圈之后，BankAccount 在这里开始真正的修改自身的数据了。但是记住，这里只是修改了内存中当前这个 BankAccount 实例的状态。持久化操作不在这里。




