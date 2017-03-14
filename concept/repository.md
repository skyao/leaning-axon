# Repository

Repository 负责提供对 Aggregates 的访问。典型的，这些参数被优化为只通过唯一标识来查找 Aggregate 。某些 repository 会存储 aggregate 的状态（例如使用ORM / Object	Relational	Mapping），而其他的会在 Event Store 中存储 aggregate 经历的状态变更。

Repository 也负责在它的后端存储中持久化施加于 aggregate 的变更。

Axon 为直接持久化 aggregate 和 event souring 提供支持。

## Axon的实现

![](images/repository-hierarchy.jpg)

- Repository： 接口定义
- AbstractRepository
- GenericJpaRepository
- EventSourcingRepository

	自动发布新的 event 到给定的 EventBus 并将 event storage 代理给提供的 EventStore

- CachingEventSourcingRepository

	在EventSourcingRepository的基础上使用缓存来改善装载性能