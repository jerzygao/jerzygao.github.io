---
layout: post
category: notes
title:  "Java命令：JPS"
tags: [java命令]
---
jps(Java Virtual Machine Process Status Tool)是JDK 1.5提供的一个显示当前所有java进程pid的命令，简单实用，非常适合在linux/unix平台上简单察看当前java进程的一些简单情况。
<!-- more --> 
***位置：***jdk的JAVA_HOME/bin/目录下面  

***原理：***java程序在启动以后，会在java.io.tmpdir指定的目录下，就是临时文件夹里，生成一个类似于hsperfdata_User的文件夹，这个文件夹里（在Linux中为/tmp/hsperfdata_{userName}/），有几个文件，名字就是java进程的pid，因此列出当前运行的java进程，只是把这个目录里的文件名列一下而已。 至于系统的参数什么，就可以解析这几个文件获得。

***参数：***

-q 只显示pid，不显示class名称,jar文件名和传递给main 方法的参数  
-m 输出传递给main 方法的参数，在嵌入式jvm上可能是null  
-l 输出应用程序main class的完整package名 或者 应用程序的jar文件完整路径名  
-v 输出传递给JVM的参数   

jps是我最常用的java命令。使用jps可以查看当前有哪些Java进程处于运行状态。如果我运行了一个web应用（使用tomcat、jboss、jetty等启动）的时候，我就可以使用jps查看启动情况。有的时候我想知道这个应用的日志会输出到哪里，或者启动的时候使用了哪些javaagent，那么我可以使用jps -v 查看进程的jvm参数情况。  

***常见错误：***   
用ps-ef|grep java能看到启动的java进程，但是用jps查看却不存在该进程的id。jconsole、jvisualvm可能无法监控该进程，其他java自带工具也可能无法使用  

***分析：*** jps、jconsole、jvisualvm等工具的数据来源就是这个文件（/tmp/hsperfdata_userName/pid)。所以当该文件不存在或是无法读取时就会出现jps无法查看该进程号，jconsole无法监控等问题  

***原因：***

1. 磁盘读写、目录权限问题 若该用户没有权限写/tmp目录或是磁盘已满，则无法创建/tmp/hsperfdata_userName/pid文件。或该文件已经生成，但用户没有读权限,该文件应与当前用户属于同一用户组  
2. 临时文件丢失，被删除或是定期清理 对于linux机器，一般都会存在定时任务对临时文件夹进行清理，导致/tmp目录被清空。这也是我第一次碰到该现象的原因。常用的可能定时删除临时目录的工具为crontab、redhat的tmpwatch、ubuntu的tmpreaper等等  
这个导致的现象可能会是这样，用jconsole监控进程，发现在某一时段后进程仍然存在，但是却没有监控信息了  
3. java进程信息文件存储地址被设置，不在/tmp目录下 上面我们在介绍时说默认会在/tmp/hsperfdata_userName目录保存进程信息，但由于以上1、2所述原因，可能导致该文件无法生成或是丢失，所以java启动时提供了参数(-Djava.io.tmpdir)，可以对这个文件的位置进行设置，而jps、jconsole都只会从/tmp目录读取，而无法从设置后的目录读取信息  
