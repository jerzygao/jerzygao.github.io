---
layout: post
categories: "solution"
title:  "由范型数组错误引发的思考-JVM如何理解范型？"
tags: [JAVA]
---
&nbsp;&nbsp;今天写代码时随手写了一段如下的代码：  
{% highlight java %}
 Map<String,String>[] marketProxyInfoMapArray = new Map<String,String>[] { map1, map2,map3 };
{% endhighlight %}
 写的时候感觉不对劲，果然编译就报错了:Cannot create a generic array of Map<String,String>，范型是不支持数组的，那么JVN到底如何处理的范型呢？
 查找资料的时候发现了这篇文章值得参考：[参考博客连接](http://hxraid.iteye.com/blog/549509)
