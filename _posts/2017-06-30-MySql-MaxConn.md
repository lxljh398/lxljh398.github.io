---
layout: post
title: "MySQL 的最大连接数"
date: 2017-07-19
categories: MySql
tags: 设置
excerpt: 在大量用户访问并发情况下，如果只是单薄的服务器环境，会出现提示超出连接数。
author: fatso
---

* content
{:toc}


## 连接数设置
通常，mysql的最大连接数默认是100, 最大可以达到16384。
1、查看最大连接数:
``` sql
show variables like '%max_connections%';
```
2、修改最大连接数

方法一：修改配置文件。推荐方法一

进入MySQL安装目录 打开MySQL配置文件 my.ini 或 my.cnf查找 max_connections=100 修改为 max_connections=1000 服务里重起MySQL即可.

方法二：命令行修改。不推荐方法二

命令行登录MySQL后。设置新的MySQL最大连接数为200：
``` shell
MySQL> set global max_connections=200。
```
这种方式有个问题，就是设置的最大连接数只在mysql当前服务进程有效，一旦mysql重启，又会恢复到初始状态。因为mysql启动后的初始化工作是从其配置文件中读取数据的，而这种方式没有对其配置文件做更改。

注意：设置最大连接值注意环境因素，iis的承载能力等等，一般2000