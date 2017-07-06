---
layout: post
title:  "Spring Boot 配置web.xml的listener"
date:   2017-4-6
categories: spring boot
comments: true
---

## Spring Boot 配置listener

要将

```xml
<listener>
  <listener-class>
   org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>
 </listener>
```

更改为Spring-Boot的配置方式具体代码如下：

```java
import org.springframework.boot.web.servlet.ServletContextInitializer;
import org.springframework.context.annotation.Configuration;

import javax.servlet.FilterRegistration;
import javax.servlet.ServletContext;

@Configuration
public class MyWebApplicationInitializer implements ServletContextInitializer {

    @Override
    public void onStartup(ServletContext sc) {
       sc.addListener(new SingleSignOutHttpSessionListener());
    }
}

```
