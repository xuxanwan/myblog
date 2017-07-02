##spring boot 

```java
	package com.infoq.springboot;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
	import org.springframework.web.bind.annotation.*;

	@RestController
	@EnableAutoConfiguration//告知Boot要采用一种特定的方式来对应用进行配置。这种方法会将其他样板式的配置均假设为框架默认的约定，因此能够聚焦于如何尽快地使应用准备就绪以便运行起来。Application类是可运行的，因此，当我们以Java Application的方式运行这个类时，就能启动该应用及其嵌入式的容器，这样也能实现即时地开发。
	public class Application {

	  @RequestMapping("/")
	  public String home() {
	    return "Hello";
	  }

	  public static void main(String[] args) {
	    SpringApplication.run(Application.class, args);
	  }
	}
```
 - [icroframeworks1-spring-boot](http://www.infoq.com/cn/articles/microframeworks1-spring-boot)