---
layout: post
category: notes
title:  "Mysql中的分区功能"
tags: [mysql]
---
分区  
当数据量大的时候仅仅使用索引是不够的，这时候可以使用Mysql的一个功能：分区  
将数据划分为多块在多个位置存放，可以是不同的磁盘也可以是不同的机器  
分区后表面还是一张表但是数据散列在不同的位置  
读写时没有任何区别Mysql自动组织分区数据  
<!-- more -->
分区的四种形式: range list hash key  

以range使用为例:  
新建分区:

~~~

alter table xxx partition  by range(id)(
partition A values less than (4),   --xxx表id为1，2，3的数据在A分区
partition B values less than (6) data DIRECTORY ='D:/data/b' INDEX DIRECTORY='D:/data/b' ,   --xxx表id为4，5 在B分区 分区路径改为其他文件夹
partition C values less than (MAXVALUE)  --其他数据在C分区
)

~~~

合并分区: `alter table 表名 reorganize partition 分区1，分区2 into (partiton 分区名 values less tan (xxx))`  
删除分区(不丢失数据): `alter table 表名 remove partitioning;`  
丢弃分区（丢失数据): `alter table 表名 drop partition;`  

从指定分区查询数据:
`select * from xxx partition(A)`  

一般按照数据类别如文章的分类，日期如文章发布日期，等指标来分区，数据量小不需要分区 一般分区区间跨度为5~10万
