---
layout: post
category: coding
title:  "使用guava RateLimter来做接口限流"
tags: [JAVA]
---
&nbsp;&nbsp;今天需要处理一个接口的限流问题，限制接口每小时访问数量，发现 guava 的Ratelimiter 可以很好的实现我的这个需求。  
<!-- more -->
常用的限流算法有两种：漏桶算法和令牌桶算法。 漏桶算法思路很简单，请求先进入到漏桶里，漏桶以一定的速度出水，当水请求过大会直接溢出，可以看出漏桶算法能强行限制数据的传输速率。

对于很多应用场景来说，除了要求能够限制数据的平均传输速率外，还要求允许某种程度的突发传输。这时候漏桶算法可能就不合适了，令牌桶算法更为适合。如图2所示，令牌桶算法的原理是系统会以一个恒定的速度往桶里放入令牌，而如果请求需要被处理，则需要先从桶里获取一个令牌，当桶里没有令牌可取时，则拒绝服务。

google开源工具包guava提供了限流工具类RateLimiter，该类基于“令牌桶算法”，非常方便使用。

[API中文文档](http://ifeve.com/guava-ratelimiter/)  
[RateLimiter使用实践](https://dzone.com/articles/ratelimiter-discovering-google)  
