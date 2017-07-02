##spring boot 事务

spring boot为基础的项目,配置了两个数据库, 配置事务处理

在数据库配置类中添加"@EnableTransactionManagement(proxyTargetClass=true)"就可以了[transactional-do-not-work-in-spring-boot](http://stackoverflow.com/questions/28304310/transactional-do-not-work-in-spring-boot)

但是两个数据库配置的时候, 又不行了,注解掉其中一个的`DataSourceTransactionManager`又可以了, 我认为很有可能是起冲突了,其中第一个数据库的datasourcemanager覆盖了另一个datasourcemanager.

验证了下, 在两个datasourcemanager的初始化里面加入了print语句,结果是只加载了一个.

怎么办呢?

- [官方transaction文档](http://docs.spring.io/spring/docs/3.0.x/spring-framework-reference/html/transaction.html#transaction-declarative-annotations)