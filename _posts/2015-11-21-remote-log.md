---
layout: post
category: solution
title:  "java远程日志"
tags: [JAVA]
---
工作中有需要查看客户端日志的情况，以前的解决办法是远程客户电脑打开日志查看日志，这样很不方便并且有些客户不希望你远程连接他的电脑。
这就需要一种把客户端日志传回我们服务器的方案，还好平时使用的日志系统log4j和llogback都支持远程日志。  
<!-- more -->
## log4j的远程日志实现  

### 客户端配置

客户端日志配置log4j.properties中加入SocketAppender的配置项
新加名称为A1的SocketAppender  
`log4j.appender.A1=org.apache.log4j.net.SocketAppender`  
配置远程日志服务器的ip为115.29.42.112，端口为6000  
`log4j.appender.A1.RemoteHost=115.29.42.112`  
`log4j.appender.A1.Port=6000`  

### 服务端配置

服务器上按照常规日志配置即可，如以下log4j_server.properties

    log4j.rootLogger=info,ConApp,FileApp
    log4j.logger.org.apache.http = warn
    log4j.logger.com.gargoylesoftware = error
    log4j.appender.ConApp=org.apache.log4j.ConsoleAppender
    log4j.appender.ConApp.layout=org.apache.log4j.PatternLayout
    log4j.appender.ConApp.layout.ConversionPattern=%p %d{yyyy-MM-dd HH:mm:ss,SSS} [%t] %c %m%n
    log4j.appender.FileApp=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.FileApp.File=/var/log/client-info.log
    log4j.appender.FileApp.DatePattern ='.'yyyy-MM-dd
    log4j.appender.FileApp.layout=org.apache.log4j.PatternLayout
    log4j.appender.FileApp.layout.ConversionPattern=%p %d{yyyy/MM/dd HH:mm:ss} [%t] %l - %m%n
    log4j.appender.FileApp.Append=true

在服务器上启动SimpleSocketServer即可，注意需要将log4j-1.2.17.jar加入到CLASSPATH中

`java org.apache.log4j.net.SimpleSocketServer 6000 log4j_server.properties`


## logback远程日志的实现


### 客户端配置

客户端logback.xml中加入以下SocketAppender配置

    <appender name="SOCKET" class="ch.qos.logback.classic.net.SocketAppender">
        <remoteHost>${host}</remoteHost>
        <port>${port}</port>
        <reconnectionDelay>10000</reconnectionDelay>
        <includeCallerData>${includeCallerData}</includeCallerData>
      </appender>

rootlogger中加入上面配置的名为SOCKET的SocketAppender

    <root level="DEBUG">
        ....
        <appender-ref ref="SOCKET" />
     </root>  


### 服务端配置


服务器端配置logback_socketserver.xml如下

    <?xml version="1.0" encoding="UTF-8" ?>
      <configuration>
            <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
                    <!-- encoders are assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder
                            by default -->
                    <encoder>
                            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} %L - %msg%n
                            </pattern>
                    </encoder>
            </appender>

            <appender name="ROLLING"
                    class="ch.qos.logback.core.rolling.RollingFileAppender">
                    <File>/var/log/coolchuan/submit-client-info.log</File>
                    <encoder>
                            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} %L - %msg%n
                            </pattern>
                    </encoder>
                    <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
                            <level>INFO</level>
                    </filter>
                    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                            <fileNamePattern>/var/log/coolchuan/submit-client-info.log.%d{yyyy-MM-dd}
                            </fileNamePattern>
                            <maxHistory>7</maxHistory>
                    </rollingPolicy>
            </appender>

            <root level="debug">
                    <appender-ref ref="STDOUT" />
                    <appender-ref ref="ROLLING" />
            </root>
      </configuration>

在服务器上启动SimpleSocketServer即可，注意需要将logback-classic-1.1.3.jar,logback-core-1.1.3.jar,slf4j-api-1.7.7.jar加入到CLASSPATH中

`java ch.qos.logback.classic.net.SimpleSocketServer 6000 logback_socketserver.xml`

[参考logback手册](http://logback.qos.ch/manual/appenders.html)
