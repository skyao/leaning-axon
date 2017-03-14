# Command Handler

Command Handler 从 repository 中获取领域对象(Aggregates)并执行他们的方法来修改他们的状态。这些 Aggregates 通常包含实际的业务逻辑，同时也因此需要负责保护他们自己的不可变量。

Aggregates 的状态变更导致 Domain Events 的生成。Domain Events 和 Aggregates 组成 domain model / 领域模型。

以下是一个 CommandHandler 的工作代码，用来处理 DepositMoneyCommand 命令：

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

而在 BankAccount 中，deposit() 方法的实现，是生成一个 MoneyDepositedEvent：

```java
@Aggregate
public class BankAccount {
	......
	public void deposit(long amount) {
        apply(new MoneyDepositedEvent(id, amount));
    }
}
```

