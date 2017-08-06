---
layout: post
category: notes
title:  "Mysql中的主从配置"
tags: [mysql]
---

### Mysql日志相关

Mysql有四种日志: 查询日志 错误日志 慢查询日志 二进制日志  
相关配置如下:  
<!-- more -->
~~~

log-output=FILE  #以文件的方式记录日志
#错误日志
log-error=xxxxx  

#查询日志  一切查询均进入
general-log=0  #0代表关闭，1代表开启 general-log=xxxx  
general-log-file #文件名

#慢查询日志
slow-query-log=1  #0代表关闭，1代表开启 log-slow-queries=xxxxx 
long_query_time=1   #这里设置秒，譬如一秒认为是慢的 则记录

~~~

#### 二进制日志

记录了所有的DDL(Create、Drop和Alter)和DML(insert、update、delete_的语句，但不包括查询的语句  
语句以事件的方式保存，描述了数据的更改过程     
往往用于灾难时数据恢复和我们的主从同步依据  

默认不开，需要手动开启  

~~~

log-bin=“xxxx”  #文件名或路径 linux下需要保证xxx路径权限需要改成mysql的 chon -R mysql:mysql xxx
binlog-do-db=xxx  #表示只对指定数据库生效,xxx是数据库名称

~~~

使用mysql的专有工具来查看这个日志文件  `mysqlbinlog xxxx`  

#### 从二进制日志文件中恢复数据

`mysqlbinlog --stop-position=xxxx  日志文件名 | mysql -u root -p`

代表从头恢复 到某个position  
也可以使用--start-position=xxxx ,代表从这个点恢复到结尾  
也可以按日期恢复  
--stop-date 和 --start-date

### 主从配置

1. 主从机器上都需要开启二进制日志  
2. 主从服务器上添加右侧配置标识身份: `server-id=xxx #随意用于标记主从数据库`
3. 主服务器上建议使用单独用户来使用主从服务 `create user 'masteruser'@'%' IDENTIFIED by 'Aa_123@#';`
   % 代表任何IP,当然你也可以设置一个IP, masteruser 就是根据你的口味设置一个用户名。设置权限: `GRANT REPLICATION SLAVE ON *.* TO 'masteruser'@'%' IDENTIFIED BY  'Aa_123@#';`
   这个用户专门用于读取主服务器的二进制文件。配置从服务器时用到
4. 从服务器配置: `change master to master_host='192.168.1.123', master_user='masteruser',master_password='Aa_123@#';` 192.168.1.123是主服务器IP,然后启用从服务器:
   ` start slave ; `

这样主从服务就配置好了
