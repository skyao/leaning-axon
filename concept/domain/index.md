# DDD概念

## 前言

DDD(领域驱动开发)的核心理念在于提供方法来管理在构建软件所涉及的复杂性，而这些复杂性是固有和不可避免的。

为了做到这一点，DDD规定：

1. 软件组件应该皆尽可能的为它要构建的领域做建模
2. 规定可以帮助实现上述目标的方法和构建块

这些构建块包括：

- 领域对象 / Domain Object
- 实体 / Entity
- 领域事件 / Domain Event
- 聚合 / Aggregate
- 聚合根 / Aggregate Root
- 存储库 / Repository


## 概念描述

### Aggregate & Aggregate Root

#### Aggregate

Aggregate 从技术上说是一个抽象概念，而不是一个具体的实物。

DDD 中 Aggregate 的标准定义：

> A DDD aggregate is a cluster of domain objects that can be treated as a single unit. An example may be an order and its line-items, these will be separate objects, but it's useful to treat the order (together with its line items) as a single aggregate.
> DDD 聚合是可以被视为单个单元的领域对象的集群。例如订单及其订单项，这些是单独的对象，但将订单（和它的订单项）视为单个聚合是很有用的。
>
> by [Martin Fowler](https://www.martinfowler.com/bliki/DDD_Aggregate.html)

Aggregate 是领域对象的逻辑集合，以形成一个原子而有粘性的整体。

这些领域对象有两个特性：

1. 从整体上说，他们可以构成一个逻辑物体
2. 从个体上说，他们每个都是独立存在

另外一个典型例子，车辆，由各种对象构成：轮胎，方向盘，发动机，座椅。

#### Aggregate Root

> An aggregate will have one of its component objects be the aggregate root. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the integrity of the aggregate as a whole.
> 聚合的组件对象中有一个被视为聚合根。来自聚合外部的任何引用应仅到达聚合根。因此根可以确保聚合的完整性。


#### 注意事项

> DDD Aggregates are sometimes confused with collection classes (lists, maps, etc). DDD aggregates are domain concepts (order, clinic visit, playlist), while collections are generic. An aggregate will often contain mutliple collections, together with simple fields. The term "aggregate" is a common one, and is used in various different contexts (e.g. UML), in which case it does not refer to the same concept as a DDD aggregate.
> DDD 聚合有时和集合类(lists,maps,等)混淆。DDD 聚合是领域概念(订单，播放列表)，而集合是通用的。聚合经常会包含多个集合，和简单字段在一起。术语 "aggregate" 是一个通用术语，并用于多种不同上下文（如UML），在这种情况下它和DDD的aggregate并不是同一个概念。

### Repository

Repository 用来抽象获取和持久化 Aggregate 的方式。





