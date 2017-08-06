---
layout: post
category: coding
title:  "内存泄露分析"
tags: [JAVA]
summary: "内存泄露分析"
---

-XX:+HeapDumpOnOutOfMemoryError  -XX:HeapDumpPath=/var/log/heapdump
>启动参数，在发生OOM时将内存映射为*.hprof 文件存放于/var/log/heapdump 目录

<!-- more -->

jmap -dump:format=b,file=/data/heapdump/dump.hprof 23308
>为进程ID为23308的java进程生成内存映射文件/data/heapdump/dump.hprof

jhat -J-Xmx6144m -port 9998 /data/heapdump/java_pid12263.hprof
>分析java_pid12263.hprof 文件并生成html报告 访问http://执行命令服务器IP:9998 可以看到内存分析报告
