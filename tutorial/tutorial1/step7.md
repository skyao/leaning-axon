# 步骤 7：选择事件序列化

可以指定序列化类型，例如示例中显示的 XML 格式的，或 JSON 格式（使用org.axonframework.serializer.json.JacksonSerializer），Java对象序列化。

事件存储使用 `serializer` 来序列化和反序列化事件对象。

```xml
<bean id="eventSerializer" class="org.axonframework.serializer.xml.XStreamSerializer"/>
```
