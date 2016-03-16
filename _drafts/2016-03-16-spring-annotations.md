# spring#
## @Component, @Service, @Repository, @Controller 区别 ##
###区别###
- 其实是个的效果是一样的.都是申明一个bean是spring管理的.具体使用场景如下:

| Annotation | Meaning|
|------------|--------|
| @Component | 通常用在spring管理的类注解上|
| @Repository| 持久层注解|
| @Service   | Service层注解|
| @Controller| Controller层注解|
###参见###
- [whats-the-difference-between-component-repository-service-annotations-in](http://stackoverflow.com/questions/6827752/whats-the-difference-between-component-repository-service-annotations-in)
- [http://javapapers.com/spring/spring-component-service-repository-controller-difference/](http://javapapers.com/spring/spring-component-service-repository-controller-difference/ "spring 注解")
- [http://blog.csdn.net/lf_software_studio/article/details/8256510](http://blog.csdn.net/lf_software_studio/article/details/8256510 "Spring注解@Component、@Repository、@Service、@Controller区别")

## spring aop
-  [Aspect Oriented Programming with SpringPrev     Part III. Core Technologies](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/aop.html) 
- [【转载】正确理解Spring AOP中的Around advice]( http://blog.163.com/chen_guangqi/blog/static/2003111492012101653052508/)


##spring mybatis  多数据库配置
- [http://stackoverflow.com/questions/18201075/mybatis-spring-multiple-databases-java-configuration](http://stackoverflow.com/questions/18201075/mybatis-spring-multiple-databases-java-configuration   )
-  druid-stat-interceptor这个干吗用的?
- 在设计框架的时候,数据库相关的mybatis层文件要独立出一个modules出来. 否则在多数据配置的时候, 系统设置就会变得非常麻烦.这方面的设置可以参考wt-workben:
    ```
src
  main
    java
      com.xxx.xxx
        modules
          db1 config
          redis config
          security config
          db2 confilg
        util
          image
          redis
        workbench
          config
          controller
          filter
          service
    resources  
      com.xxx.xxx.modules
        db1 mapper
        db2 mapper
        application.yml
        logback.xml
        banner.txt
 yo
```

----------
### spring 配置文件

- `org.springframework.web.servlet.view.InternalResourceViewResolver`作用
  - 实例:
```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix">
        <value>/WEB-INF/pages/</value>
    </property>
    <property name="suffix">
        <value>.jsp</value>
    </property>
</bean>
```
  - 类结构
  ![InternalResourceViewResolver类结构](27e24b76-c437-41b4-ace2-7846f84a4168_128_files/RKW4SJ5_2410XZ_5BZ8BG2B69UH.png "InternalResourceViewResolver类结构")
  - 作用
  根据模板名称和位置解析视图。InternalResourceViewResolver视图解析器的策略是直接映射到模板名称和位置。这个解析器能够支持Redirect前缀（如："redirect:welcome"）。




