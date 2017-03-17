# Axon

## 概况

下图展示了 Axon 中基于 CQRS 的事件驱动架构的例子。

显示在左边的UI模块，以两种方式和应用的其他部分交互：

1. 发送命令/Command给应用： 见上面部分
2. 向应用发起查询：见下半部分

![overview](images/detailed-architecture-overview.png)

## 实例流程

我们以一个最简单的业务流程来讲解各个环节： 假设我们有一个银行账户 BankAccount，我们现在需要从账户上扣除一定金额，做一个 deposit 操作。

> 源代码请见 Axon 官方给出的 AxonBank 这个demo。



