---
layout: post
title:  "Spring Boot 配置web.xml中的filter"
date:   2017-4-6
categories: Spring Boot
---

## Spring Boot 配置web.xml中的filter

使用spring boot后，没有显示的web.xml文件，现在拦截器的配置方式更改如下：
原有的web.xml配置文件为：

```xml
<filter>
  <filter-name>CASLogoutFilter</filter-name>
  <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>
 </filter>
 <filter-mapping>
  <filter-name>CASLogoutFilter</filter-name>
  <url-pattern>/*</url-pattern>
 </filter-mapping>
```

现在为：

```java
import org.springframework.boot.web.servlet.ServletContextInitializer;
import org.springframework.context.annotation.Configuration;

import javax.servlet.FilterRegistration;
import javax.servlet.ServletContext;

@Configuration
public class MyWebApplicationInitializer implements ServletContextInitializer {

    @Override
    public void onStartup(ServletContext sc) {
        FilterRegistration.Dynamic cASLogoutFilter = sc.addFilter("CASLogoutFilter", SingleSignOutFilter.class);
        cASLogoutFilter.setInitParameter("casServerUrlPrefix", "http://127.0.0.1:8080");
        cASLogoutFilter.addMappingForUrlPatterns(null, false, "/*");
    }
}
```

