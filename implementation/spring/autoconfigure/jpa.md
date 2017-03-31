# JpaConfiguration


## 激活条件

JpaConfiguration 激活的条件是存在 CommandBus 是 `EntityManagerFactory` 的bean：

```java
@ConditionalOnBean(EntityManagerFactory.class)
@RegisterDefaultEntities(packages = {"org.axonframework.eventsourcing.eventstore.jpa",
        "org.axonframework.eventhandling.tokenstore",
        "org.axonframework.eventhandling.saga.repository.jpa"})
@Configuration
public static class JpaConfiguration {
......
}
```

## 自动配置的内容

### EventStorageEngine

自动配置的 EventStorageEngine 是 `JpaEventStorageEngine`：

```java
@ConditionalOnMissingBean
@Bean
public EventStorageEngine eventStorageEngine(Serializer serializer,
	PersistenceExceptionResolver persistenceExceptionResolver,
	AxonConfiguration configuration,
	EntityManagerProvider entityManagerProvider,
	TransactionManager transactionManager) {
    return new JpaEventStorageEngine(serializer, configuration.getComponent(EventUpcaster.class), persistenceExceptionResolver, null, entityManagerProvider, transactionManager, null, null, true);
}
```

### SagaStore

自动配置的 SagaStore 是 `JpaSagaStore`：

```java
@ConditionalOnMissingBean(SagaStore.class)
@Bean
public JpaSagaStore sagaStore(Serializer serializer, EntityManagerProvider entityManagerProvider) {
    return new JpaSagaStore(serializer, entityManagerProvider);
}
```

