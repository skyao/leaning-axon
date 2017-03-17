# Event Handler

## 介绍

Event Handler 接收并处理事件。

一些 Handler 将更新用于查询的数据源，而另外一些发送消息到外部系统。

你可能会注意到，command handle 完全不知道哪些组件会对他们做出的变更感兴趣。这意味着可以以非侵入性的方式来用新功能扩展应用程序。所有需要做的只是添加另一个 Event Handler。事件耦合了应用程序中的所有组件。

在某些情况下，事件处理要求发送新的 command 到应用。例如，当接收到订单时，这意味着客户的账号需要扣除购买的总价，而物流必须被告知要准备被购买货物的送货。

在很多应用中，逻辑比这个更加的复杂：如果客户没有及时支付？ 应该立即发送给物流，还是等先完成支付？ saga 是CQRS 中负责管理这些复杂业务实物的概念。

## Axon

在 Axon 中，在 spring 下定义 Event handler 的方式非常简单，只要在 spring bean 的方法上加上 @EventHandler 注解即可，如前面见过的 BankAccount 上的 Event Handler 实现:

```java
@EventHandler
public void on(MoneyAddedEvent event) {
    balanceInCents += event.getAmount();
}
```

同一个 Domain Event 是可以发送给多个 Event Handler 的，比如肯定需要有一个 Event Handler 来处理上面的 MoneyAddedEvent，然后将修改之后的数据保存起来，如类 BankAccountEventListener：

```java
@Component
public class BankAccountEventListener {
	......
    @EventHandler
    public void on(MoneyAddedEvent event) {
    	// 
        BankAccountEntry bankAccountEntry = repository.findOneByAxonBankAccountId(event.getBankAccountId());
        bankAccountEntry.setBalance(bankAccountEntry.getBalance() + event.getAmount());

        repository.save(bankAccountEntry);

        broadcastUpdates();
    }
}
```

有两个 repository

1. BankAccountRepository

	这个是 org.springframework.data.repository

	用于 BankAccountEventListener, 用来存取 BankAccountEntry 对象

	用于 BankAccountController， 用于查询 BankAccountEntry

2. Repository< BankAccount >

	这个是 import org.axonframework.commandhandling.model.Repository

	用于 BankAccountCommandHandler， 用于通过 load(aggregateIdentifier) 方法来装载 Aggreate 对象

	构造方式： `axonConfiguration.repository(BankAccount.class)`


