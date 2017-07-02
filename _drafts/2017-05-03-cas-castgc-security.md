---
layout: post
title:  "CAS 之 CASTGC"
date:   2017-05-03
categories: CAS
---
## CAS 

CAS(central authentication service)是一个web的单点登录协议.它的目的是让用户只要一次登录在多个互相信任的应用中进行访问.

### CASTGC是什么
CASTGC是Cookie中的键名,其中存储有TGC,CASTGC代表着一次单点登录会话.用户第一次登录后,CAS会在对应Portal的域中添加该Cookie.需要单点登录操作时,SSO检测CASTGC并根据CASTGC获取TGT,从而知道用户的登录会话是否存在.这一就会有一个问题,若是在传输的过程中,CASTGC被其他用户截取,会不会导致登录被人破解,接下来就从这方面考虑考虑.

### 安全性保证
CAS协议设计时对TGT设计有
1. CASTGC对应的

### 三,参考网址

* 1.[utf-8 wikipedia](https://zh.wikipedia.org/wiki/UTF-8)

