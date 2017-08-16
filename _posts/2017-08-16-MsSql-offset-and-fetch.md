---
layout: post
title: "offset and fetch 分页"
date: 2017-08-16 15:06:05
categories: MsSql
tags: 新特性 优化 SQLServer
excerpt: offset and fetch 新特性的认识。
author: fatso
---

* content
{:toc}


## offset and fetch
在sql分页中常用的就是ROW_NUMBER() ，在sql2012中推出新的特性offset and fetch，这里做一对比

### 方法一：ROW_NUMBER() 

在 Sql Server 2000 之后的版本中，ROW_NUMBER() 这种分页方式一直都是很不错的，比起之前的游标分页，性能好了很多，因为 ROW_NUMBER() 并不会引起全表扫表，但是，语法比较复杂，并且，随着页码的增加，性能也越来越差。 

#### 语法 ：
``` sql
ROW_NUMBER ( ) OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )
```
#### 实例：
``` sql
select *,ROW_NUMBER() OVER(Order By ID) as rowid into #temp_users from [AlphaWallet.Api_Main_Stress].[dbo].[Users] 

select * from #temp_users where rowid between (31) and 45

drop table #temp_users
```



### 方法二：offset and fetch

在Sql Server 2012 中 offset and fetch 的新特性，发现 offset and fetch 无论语法的简洁还是功能的强大，都是相当相当不错的。

注意：
通过 OFFSET-FETCH 子句，您可以从结果集中仅提取某个时间范围或某一页的结果。OFFSET-FETCH 只能与 ORDER BY 子句一起使用。


#### 语法 ：
``` sql
1.[ORDER BY { order_by_expression [ ASC | DESC ] } [ ,...n][<offset_fetch>] ]

2.OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }    FETCH { FIRST | NEXT } { integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY
```

    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }
        指定在从查询表达式中开始返回行之前，将跳过的行数。OFFSET 子句的参数可以是大于或等于零的整数或表达式。ROW 和 ROWS 可以互换使用。
    FETCH { FIRST|NEXT } <行计数表达式> { ROW|ROWS } ONLY
        指定在处理 OFFSET 子句后，将返回的行数。FETCH 子句的参数可以是大于或等于 1 的整数或表达式。ROW 和 ROWS 可以互换使用。同样，FIRST 和 NEXT 可以互换使用。


    使用 OFFSET-FETCH 中的限制
        ORDER BY 是使用 OFFSET 和 FETCH 子句所必需的。
        OFFSET 子句必须与 FETCH 一起使用。永远不能使用 ORDER BY … FETCH。
        TOP 不能在同一个查询表达式中与 OFFSET 和 FETCH 一起使用。
        OFFSET/FETCH 行计数表达式可以是将返回整数值的任何算术、常量或参数表达式。该行计数表达式不支持标量子查询。

#### 官方示例：

下面的示例说明如何将 OFFSET-FETCH 子句与 ORDER BY 一起使用。
示例 1- 从排序的结果集中跳过前 10 行并且返回剩余行。
``` sql
SELECT First Name + ' ' + Last Name FROM Employees ORDER BY First Name OFFSET 10 ROWS;
```
示例 2- 从排序的结果集中跳过前 10 行并且返回接下来的 5 行。
``` sql
SELECT First Name + ' ' + Last Name FROM Employees ORDER BY First Name OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```


#### 个人实例：
``` sql
select * from [AlphaWallet.Api_Main_Stress].[dbo].[Users] order by id OFFSET 30 ROW FETCH NEXT 15 rows only
```