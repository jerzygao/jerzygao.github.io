---
layout: post
category: solution
title:  "解决UI4Jwebkit部署在linux服务器出现No toolkit found运行时异常"
tags: [JAVA]
---
&nbsp;&nbsp;使用UI4Jwebkit时部署在linux服务器出现No toolkit found运行时异常。异常信息如下：
<!-- more -->
{% highlight java %}
java.lang.RuntimeException: No toolkit found
        at com.sun.javafx.tk.Toolkit.getToolkit(Toolkit.java:210) ~[jfxrt.jar:na]
        at com.sun.javafx.application.PlatformImpl.isFxApplicationThread(PlatformImpl.java:264) ~[jfxrt.jar:na]
        at javafx.application.Platform.isFxApplicationThread(Platform.java:99) ~[jfxrt.jar:na]
 {% endhighlight %}
 &nbsp;&nbsp;要解决此问题首先需要将jre/lib/ext/jfxrt.jar移动到jre/lib 下，然后参照UI4J 官方说明解决，[官方说明](https://github.com/ui4j/ui4j),摘录如下：
>Supported Platforms
Ui4j has been tested under Windows 8.1 and Ubuntu 14.04, but should work on any platform where a Java 8 JRE or JDK is available.
## Headless Mode
Ui4j can be run in "headless" mode using Xfvb or with using Monocle.
## Headless Mode with Xfvb
Sample configuration for ubuntu running with headless mode:
First install these packages: sudo apt-get install xvfb x11-xkb-utils libxrender-dev libxtst-dev libgtk2.0-0
Then start the xvfb nohup xvfb :99 -ac &
Then export the DISPLAY variable export DISPLAY=:99.0
## Headless Mode with Monocle
Download or add the maven dependency of latest openjfx-monocle
Add -Dui4j.headless Java system parameter from command line or with using api System.setProperty("ui4j.headless", "true");
monocle dependency:
{% highlight xml %}
<dependency>
    <groupId>org.jfxtras</groupId>
    <artifactId>openjfx-monocle</artifactId>
    <version>1.8.0_20</version>
    <scope>test</scope>
</dependency>
{% endhighlight %}

个人感觉使用Headless Mode with Monocle比较简单。
