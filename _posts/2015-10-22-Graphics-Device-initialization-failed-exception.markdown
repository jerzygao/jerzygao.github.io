---
layout: post
category: solution
title:  "使用UI4J的java应用在linux服务器启动出现Graphics Device initialization failed解决办法"
tags: [JAVA,linux,ui4j]
---
&nbsp;&nbsp;使用UI4J的java应用在linux服务器启动出现Graphics Device initialization failed异常，异常堆栈如下：
<!-- more -->  
{% highlight java %}
Graphics Device initialization failed for :  es2, sw
Error initializing QuantumRenderer: no suitable pipeline found
java.lang.RuntimeException: java.lang.RuntimeException: Error initializing QuantumRenderer: no suitable pipeline found
        at com.sun.javafx.tk.quantum.QuantumRenderer.getInstance(QuantumRenderer.java:300)
        at com.sun.javafx.tk.quantum.QuantumToolkit.init(QuantumToolkit.java:244)
        at com.sun.javafx.tk.Toolkit.getToolkit(Toolkit.java:179)
        at com.sun.javafx.application.PlatformImpl.startup(PlatformImpl.java:210)
        at com.sun.javafx.application.LauncherImpl.startToolkit(LauncherImpl.java:653)
        at com.sun.javafx.application.LauncherImpl.launchApplicationWithArgs(LauncherImpl.java:314)
        at com.sun.javafx.application.LauncherImpl.launchApplication(LauncherImpl.java:305)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:483)
        at sun.launcher.LauncherHelper$FXHelper.main(LauncherHelper.java:767)
Caused by: java.lang.RuntimeException: Error initializing QuantumRenderer: no suitable pipeline found
        at com.sun.javafx.tk.quantum.QuantumRenderer$PipelineRunnable.init(QuantumRenderer.java:98)
        at com.sun.javafx.tk.quantum.QuantumRenderer$PipelineRunnable.run(QuantumRenderer.java:128)
        at java.lang.Thread.run(Thread.java:744)
Exception in thread "main" java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:483)
        at sun.launcher.LauncherHelper$FXHelper.main(LauncherHelper.java:767)
Caused by: java.lang.RuntimeException: No toolkit found
        at com.sun.javafx.tk.Toolkit.getToolkit(Toolkit.java:191)
        at com.sun.javafx.application.PlatformImpl.startup(PlatformImpl.java:210)
        at com.sun.javafx.application.LauncherImpl.startToolkit(LauncherImpl.java:653)
        at com.sun.javafx.application.LauncherImpl.launchApplicationWithArgs(LauncherImpl.java:314)
        at com.sun.javafx.application.LauncherImpl.launchApplication(LauncherImpl.java:305)
{% endhighlight %}
 报这个错误是因为linux服务器没有安装libswt-gtk-3-java and gkt3

 命令：sudo apt-get install libswt-gtk-3-jni libswt-gtk-3-java

 安装完后可解决此问题

 查找资料的时候发现了这几篇文章值得参考：

 [参考连接askubuntu](http://askubuntu.com/questions/125150/unsatisfied-link-error-and-missing-so-files-when-starting-eclipse)

 [参考连接stackoverflow](http://stackoverflow.com/questions/21185156/javafx-on-linux-is-showing-a-graphics-device-initialization-failed-for-es2-s)

 notoolkit 解决办法参照上一篇blog[解决UI4Jwebkit部署在linux服务器出现No toolkit found运行时异常](http://54gaozhe.com/java/no-toolkit-found-error.html)
