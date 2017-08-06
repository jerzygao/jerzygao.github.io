---
layout: post
category: notes
title:  "Mysql中的表锁和行锁"
tags: [mysql]
---
## 表级锁

锁住整张表 其他事物进不来  
myisam innodb都支持表级锁  
<!-- more -->
### 读锁  lock table xxx read

当前会话不可写  
其他会话 不可写 可读   

### 写锁 lock table xxx read

当前会话可写 可读  
其他会话 不可写 不可读

### 解锁unlock tables

把当前会话所有锁住的表解锁，如果忘记解锁 当再次加锁时会自动释放上一次的锁

## 行锁

行锁 最小粒度锁，真正的事物锁  
只有innodb支持

> Mysql事物支持:使用 start TRANSACTION


行锁是索引级别不是记录级别的

> 如果加锁的sql语句where条件中没有用到索引，则会导致锁住整张表  
> 加锁sql语句中给索引加锁，但是其他会话更新操作没有使用索引字段同样无法更新  
> 索引无论加锁会话还是更新会话都需要使用索引字段  

### 共享锁 select aaa from xxxx where c=d  LOCK IN SHARE MODE  

select 出来的行数据只有当前会话可修改，直至commit或rollback之后其他会话才能修改，但是加锁过程中其他会话可读

### 排他锁 select aaa from xxxx where c=d  FOR UPDATE  

其他会话依然不可写但是可以普通读，再次加锁会产生冲突
