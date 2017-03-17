# Command Bus

Command Bus 接收 Command 并将它们路由给 Command Handler. 每个 Command Handler 负责一个特定的 Command 类型并根据 command 的内容执行操作逻辑。

Command Handler 可以在 command bus 上订阅和退订特定类型的command。一种类型的命令只能被一种handler订阅。

在某些情况下，也可以执行操作逻辑而不管实际的命令类型，如验证、日志或认证。

## Axon的实现

Axon 中 CommandBus 的实现类和继承结构：

![](images/command-bus-hierarchy.jpg)


- CommandBus

	接口定义

- RecordingCommandBus

    不在订阅或者分配命令时执行任何操作，只做记录。

- SimpleCommandBus

	CommandBus 的实现，分配命令给订阅到特定命令类型的handler。可以配置拦截器来添加对命令的处理，而不管他们的类型，例如日志，安全（认证），sla监控，等等。

- AsynchronousCommandBus

	SimpleCommandBus的特殊版本，异步处理命令。默认使用Cached Thread Pool，也可以传入指定的 Executor。

- DisruptorCommandBus

	性能非常高的异步 CommandBus 实现。

- DistributedCommandBus

	分布式的 CommandBus 实现，由多个 CommandBus 的实例组成，并一起工作来分担负载。





