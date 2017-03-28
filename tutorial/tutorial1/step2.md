# 步骤2：设计命令和处理器

命令是包含数据的对象,用于更改系统状态的; 他们不返回结果。

为了管理购物车，定义两个命令：

- AddItemCommand - 将一定数量的商品（在这种例子中是植物）添加到购物车。异步发送。
- RemoveItemCommand - 删除购物车中已经存在的对象，更改库存的可用性。异步发送，因为系统不需要向用户返回任何应答。

命令处理器是 ShoppingCart（AggregateRoot）类中包含的被注解的方法，ShoppingCart代表主要领域对象。（注解允许使用 AggregateRoot 作为简单的 `Plain Old Java Object` 或 `POJO`）。事件将在相关的命令处理器程序方法中生成。

```java
@CommandHandler
public ShoppingCart(AddItemCommand command) {
    id=command.getItemId();
	// 这里写业务逻辑
    //....
    apply(new ItemAddedEvent(command.getItemId()));
}

@CommandHandler
public ShoppingCart(RemoveItemCommand command) {
    id=command.getItemId();
    // 这里写业务逻辑
    //....
    apply(new ItemRemovedEvent(command.getItemId()));
}
```

在某些场景下，无论命令的具体类型如何，您都可能需要执行其他操作（例如验证，日志记录或授权）。这可以通过命令处理拦截器来实现。这些拦截器可以在处理命令之前和之后采取行动。它们也可以完全阻止命令处理。拦截器必须实现接口`org.axonframework.commandhandling.CommandHandlerInterceptor`。

