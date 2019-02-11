---
title: MySQL profiling 最佳实践
date: 2018-12-26 12:00:00
tags:
  - MySQL
categories: 最佳实践
---

# using explain

official document: <https://dev.mysql.com/doc/refman/8.0/en/explain.html>
reference: <https://segmentfault.com/a/1190000010293791>

| 列名          | 说明                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| id            | 执行编号，标识select所属的行,每行都将显示1,内层的select语句一般会顺序编号                                     |
| select_type   | 显示本行是简单或复杂select。如果查询有任何复杂的子查询，则最外层标记为PRIMARY（DERIVED、UNION、UNION RESUlT） |
| table         | 访问引用哪个表（引用某个查询，如“derived3”）                                                                  |
| type          | 数据访问/读取操作类型（ALL、index、range、ref、eq_ref、const/system、NULL）                                   |
| possible_keys | 显示mysql可能利用到的索引                                                                                     |
| key           | 显示mysql实际采用的索引                                                                                       |
| key_len       | 显示mysql在索引里使用的字节数                                                                                 |
| ref           | 显示了之前的表在key列记录的索引中查找值所用的列或常量, 如 const                                               |
| rows          | 为了找到所需的行而需要读取的行数，估算值，不精确。通过把所有rows列值相乘，可粗略估算整个查询会检查的行数      |
| Extra         | 额外信息，如using index、filesort等                                                                           |

## 按列解释

### select_type

| select_type        | description                                   |
| ------------------ | --------------------------------------------- |
| SIMPLE             | 简单的SELECT，不实用UNION或者子查询。         |
| PRIMARY            | 最外层SELECT。                                |
| UNION              | 第二层，在SELECT之后使用了UNION。             |
| DEPENDENT UNION    | UNION语句中的第二个SELECT，依赖于外部子查询。 |
| UNION RESULT       | UNION的结果。                                 |
| SUBQUERY           | 子查询中的第一个SELECT。                      |
| DEPENDENT SUBQUERY | 子查询中的第一个SELECT，取决于外面的查询。    |
| DERIVED            | 导出表的SELECT（FROM子句的子查询）            |

### possible_keys

possible_keys
1） all：全表扫描。
2） const：读常量，最多只会有一条记录匹配，由于是常量，实际上只须要读一次。
3） eq_ref：最多只会有一条匹配结果，一般是通过主键或唯一键索引来访问。
4） fulltext：进行全文索引检索。
5） index：全索引扫描。
6） index_merge：查询中同时使用两个（或更多）索引，然后对索引结果进行合并（merge），再读取表数据。
7） index_subquery：子查询中的返回结果字段组合是一个索引（或索引组合），但不是一个主键或唯一索引。
8） rang：索引范围扫描。
9） ref：Join语句中被驱动表索引引用的查询。
10） ref_or_null：与ref的唯一区别就是在使用索引引用的查询之外再增加一个空值的查询。
11） system：系统表，表中只有一行数据；
12） unique_subquery：子查询中的返回结果字段组合是主键或唯一约束。