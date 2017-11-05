---
layout: post
title:  "jar包运行开启jmx监控"
date:   2017-11-05
categories: Java,jmx
comments: true

---
### 简介
 - 项目运行时，想要通过jvisualvm来观察内存占用情况，这时候可以通过指定java启动参数，来开启jmx。

### 命令详情
```
java -Dcom.sun.management.jmxremote   -Dcom.sun.management.jmxremote.port=9010   -Dcom.sun.management.jmxremote.local.only=false   -Dcom.sun.management.jmxremote.authenticate=false   -Dcom.sun.management.jmxremote.ssl=false   -Djava.rmi.server.hostname=127.0.0.1   -jar httpUtil.jar
```
注意,`-Dcom.sun.management.jmxremote.authenticate=false`命令使任何人都可以连接jmx。
### 参考链接
- [how-to-activate-jmx-on-my-jvm-for-access-with-jconsole](https://stackoverflow.com/questions/856881/how-to-activate-jmx-on-my-jvm-for-access-with-jconsole)