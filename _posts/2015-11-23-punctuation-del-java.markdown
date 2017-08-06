---
layout: post
category: solution
title:  "java去掉字符串中的标点"
tags: [JAVA]
---
&nbsp;&nbsp;java去掉字符串中的标点：
<!-- more -->
{% highlight java %}
  String ss = "撒的很快就好sss,sdsa。sadas.事实上、？！“”：；撒旦撒#￥%……&@）（&……订单!@#$%^^&&*()<>?:\"{}~[];'<>?'";
  System.out.println(ss.replaceAll("[\\pP‘’“”]", ""));
  //输出结果：撒的很快就好ssssdsasadas事实上撒旦撒￥订单$^^<>~<>
{% endhighlight %}
在这里利用的是Unicode编码，Unicode 编码并不只是为某个字符简单定义了一个编码，而且还将其进行了归类。

\pP 其中的小写 p 是 property 的意思，表示 Unicode 属性，用于 Unicode 正表达式的前缀。

大写 P 表示 Unicode 字符集七个字符属性之一：标点字符。
其他六个是<br>
L：字母；<br>
M：标记符号（一般不会单独出现）；<br>
Z：分隔符（比如空格、换行等）；<br>
S：符号（比如数学符号、货币符号等）；<br>
N：数字（比如阿拉伯数字、罗马数字等）；<br>
C：其他字符<br>


转载自：[日暮掩柴扉的专栏](http://blog.csdn.net/welcome000yy/article/details/7824429)
