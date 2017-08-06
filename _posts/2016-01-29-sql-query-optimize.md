---
layout: post
category: notes
title:  "Mysql中的查询优化"
tags: [mysql]
---
## explain命令

用于分析查询语句的执行情况接成本预估  
使用方法:  
<!-- more -->
~~~ sql

explain select aaa from xxx where id=5;

~~~

查询结果中有一个type字段,此字段的值可作为我们优化sql查询的关键指标  
type指标中查询效果由好到坏依次为:  
system const eq_ref ref fulltxt ref_or_null index_merge unique_subquery index_subquery range index ALL  
const 根据主键或唯一索引取出确定的一行数据。最快的一种 system为const的特例优化时暂不考虑  
fulltxt 全文索引,后面会写到全文索引的基本用法  
range 索引或主键在某一范围内  
index 仅仅只有索引被扫描  
ALL 为全表扫描，应尽量避免  
平时写sql查询时应避免全表扫描,尽量使其type为range以保证查询效率

## 使用limit分页时需要注意的地方

通过limit进行查询时即使使用了索引当数据量很大时也会导致查询缓慢  
`select aaa from xxx order by id  limit 2000,20` 的执行时间是 limit 0,20 的10几倍， limit 20000,20 的话更慢。  
上述语句 type 为index  
在where条件中规定id的范围使其type指标变为range可加快查询速度  
利用上一次的查询结果构造下一次的查询条件  
如第二页的查询方法原来的sql语句为:`select aaa from xxx order by id limit 20 ,20`  
可使用下面的sql语句优化:

~~~ sql

set @getid=(select id from xxx order by id limit(20,1));
select aaa from xxx where id>=@getid order by id  limit 20 ;

~~~

此时type为range 虽然第一步查询稍慢 但是比原来强很多.

## like 以及全文索引的使用

`select aaa from xxx where name like 'gaozhe%' order by id limit 0,20`  
此时type指标虽然为index 但是在数据量大的时候依旧很慢，原因是type挂在了 order by 语句上 id为索引.  
如下sql去掉 order by 此时type为ALL  
`select aaa from xxx where name like 'gaozhe%' limit 0,20`  
当为name字段加索引后运行以上查询，type为 range,但是百分号必须在like条件字符串的后面或者中间  
如果百分号放在了开头 如：`select aaa from xxx where name like '%gaozhe' limit 0,20 `,此时type变成了ALL，应该避免此种用法  

### fulltxt全文索引

可以为name字段加full txt全文索引，用全文索引需要在配置文件中增加全文索引配置,具体配置可参考手册    
其中 ft_min_word_len 表示索引最小长度，默认是4支持中文应该改成2或1不然长度小于4的词无法被索引  
基本用法: `select aaa from xxx where match(name) against('gaozhe') limit 0,20`  
此条sql type 为 fulltxt 效果比range还好  

但是一般生产环境不使用全文索引，而是推荐使用第三方工具如：Coreseek  
