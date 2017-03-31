# Command Bus

Axon 中 CommandBus 的实现类和继承结构：

![](../../concept/axon/images/command-bus-hierarchy.jpg)

### CommandBus

CommandBus 的接口定义：


```java
package org.axonframework.commandhandling;

public interface CommandBus {

	// 将指定命令分派到订阅到这个命令类型的 command handler
	default <C> void dispatch(CommandMessage<C> command) {
    	// 默认实现，CommandCallback 设置为LoggingCallback，仅仅打印日志
        dispatch(command, LoggingCallback.INSTANCE);
    }

    // 将指定命令分派到订阅到这个命令类型的 command handler。
    // 当方法返回时，CommandBus 实现唯一能提供的保证，是命令已经被成功接收。
    // 强烈建议实现在这个调用返回前执行基本的命令验证。
    //
    // 在开始分配命令前，实现必须启动一个 UnitOfWork ，然后要不提交要不回滚，对应于执行成功或者失败。
    <C, R> void dispatch(CommandMessage<C> command, CommandCallback<? super C, R> callback);

    // 将给定 handler 订阅到命令类型。
    // 如果给定类型已经存在订阅，则行为未定义。实现可能抛出一个异常来拒绝重复订阅，或者决定是已存还是新的handler获得订阅。
    Registration subscribe(String commandName, MessageHandler<? super CommandMessage<?>> handler);
}

```

### RecordingCommandBus

不在订阅或者分配命令时执行任何操作，只是记录他们。

这个实现不是mock的替代品，而是在很多简单场景下有用。

> TBD: 看看到底可以怎么用

### SimpleCommandBus

CommandBus 的实现，分配命令给订阅到特定命令类型的handler。可以配置拦截器来添加对命令的处理，而不管他们的类型，例如日志，安全（认证），sla监控，等等。

这个类可以被监控，如 StatisticsProvider 接口的实现所示。

> TBD: 监控？

### AsynchronousCommandBus

SimpleCommandBus的特殊版本，异步处理命令。默认使用Cached Thread Pool，也可以传入指定的 Executor。

### DisruptorCommandBus

性能非常高的异步 CommandBus 实现。

> TBD: 需要详细研究

### DistributedCommandBus

CommandBus 实现，由多个 CommandBus 的实例组成，一起工作来


