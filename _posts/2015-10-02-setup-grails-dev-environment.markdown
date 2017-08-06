---
layout: post
category: "notes"
title:  "mac/linux下搭建grails开发环境"
tags: [grails]
---
&nbsp;&nbsp;最近想进行一个side project决定使用grails开发，以前用windows，最近开始用MAC开发需要重新搭建开发环境，这里稍微介绍一下。  
<!-- more -->
&nbsp;&nbsp;首先需要下载并安装grails，grails官网下载地址为<https://grails.org/download.html>里面介绍推荐使用sdkman命令安装，当然要使用sdkman命令需要先安装sdkman,安装方式在官网是这样描述的：

>SDKMAN! (The Software Development Kit Manager)
This tool makes installing Grails on any Bash platform (Mac OSX, Linux, Cygwin, Solaris or FreeBSD) very easy.
Simply open a new terminal and enter:  

###### $ curl -s get.sdkman.io | bash  

Follow the instructions on-screen to complete installation.  
Open a new terminal or type the command:  

###### $ source "$HOME/.sdkman/bin/sdkman-init.sh"  

Then install the latest stable Grails:  

###### $ sdk install grails  

After installation is complete and you've made it your default version, test it with:  

###### $ grails -version  

That's all there is to it!  

安装过程很简单命令：curl -s get.sdkman.io | bash 安装sdkman安装完成后另开一个终端或者在原有终端执行source "$HOME/.sdkman/bin/sdkman-init.sh 执行完毕后可以直接使用命令sdk install grails安装grails，但是在国内有个问题那就是墙的问题导致下载grails-3.0.8.zip这个文件时总是下载不下来导致安装失败。我是这样解决的：

1.  grails-3.0.8.zip在官网下载压缩包　
2.  将压缩包放到当前用户根目录下.sdkman文件夹下archives文件夹里，例如我就放在/Users/gaozhe/.sdkman/archives中
3.  执行sdk install grails安装grails

不翻墙的话下载比较缓慢，我把我下载的文件放在<http://pan.baidu.com/s/1o6vMXJk>中。下载完grails需要下载开发IDE因为没买收费的IDEA我就使用eclipse开发，spring有插件集成版的eclipse在<http://spring.io/tools/ggts/all>可以下载Groovy/Grails Tool Suite，各个平台的版本都有。使用下载的ggts进行grails的开发还是比较便捷的。ggts下载速度比较快这里不给网盘链接了。

这样grails开发环境就算完成了。
