# 步骤 3：定义事件和监听器

一旦命令处理被执行，工作单元就被提交，所有操作完成。 在这个点上，每个存储库都被通知状态更改，注册的事件被发送到事件总线进行发布。如果您为此事件提供监听器，则它将应命令和事件生成而异步运行。

在我的简单应用中，有两个事件生成：

- ItemAddedEvent (by AddItemCommand)
- ItemRemovedEvent (by RemoveItemCommand)

这些事件包含与事件处理相关的数据(例如，itemId, 价格和数量).

在 `ShoppingCartEventListener` 类中，我为每个事件注册监听器.

```java
public class ShoppingCartEventListener {

    @EventHandler
    public void onEvent(ItemAddedEvent event) {
            System.out.println("Received ItemAddedEvent id:" + event.getItemId() + " on thread named " + Thread.currentThread().getName());
        }

    @EventHandler
    public void onEvent(ItemRemovedEvent event) {
            System.out.println("Received ItemRemovedEvent id:" + event.getItemId() + " on thread named " + Thread.currentThread().getName());
        }

    }
```

这是放置代码来更新数据视图的正确位置，查询将作用于该视图。根据监听器的目的，数据的持久化可能在多个数据库上完成。每个数据库可能不仅具有单独的查询模型，还可以使用不同的技术（如SQL DB，NoSQL DB等）。
