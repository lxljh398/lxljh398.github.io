---
layout: post
title: "Python 格式化输出"
date: 2019-05-07 19:06:05
categories: Python
tags: Python-100Days Basis Output
excerpt: 基础语法，格式化输出，占位符使用
author: fatso
---

* content
{:toc}


## 字符串格式化代码：

格式	|   描述
----|----
%%	|   百分号标记
%c	|   字符及其ASCII码
%s	|   字符串
%d	|   有符号整数(十进制)
%u	|   无符号整数(十进制)
%o	|   无符号整数(八进制)
%x	|   无符号整数(十六进制)
%X	|   无符号整数(十六进制大写字符)
%e	|   浮点数字(科学计数法)
%E	|   浮点数字(科学计数法，用E代替e)
%f	|   浮点数字(用小数点符号)
%g	|   浮点数字(根据值的大小采用%e或%f)
%G	|   浮点数字(类似于%g)
%p	|   指针(用十六进制打印值的内存地址)
%n	|   存储输出字符的数量放进参数列表的下一个变量中

## 案例：

### 1.打印字符串
``` py
print("My name is %s" %("Alfred.Xue"))
#输出效果：
# My name is Alfred.Xue
```

### 2.打印整数
``` py
print("I am %d years old." %(25))
#输出效果：
# I am 25 years old.
```

### 3.打印浮点数
``` py
print ("His height is %f m"%(1.70))
#输出效果：
# His height is 1.700000 m
```

### 4.打印浮点数（指定保留两位小数）
``` py
print ("His height is %.2f m"%(1.70))
#输出效果：
# His height is 1.70 m
```

### 5.指定占位符宽度
``` py
print ("Name:%10s Age:%8d Height:%8.2f"%("Alfred",25,1.70))
#输出效果：
# Name:    Alfred Age:      25 Height:    1.70
```

### 6.指定占位符宽度（左对齐）
``` py
print ("Name:%-10s Age:%-8d Height:%-8.2f"%("Alfred",25,1.70))
#输出效果：
# Name:Alfred     Age:25       Height:1.70
```

### 7.指定占位符（只能用0当占位符？）
``` py
print ("Name:%-10s Age:%08d Height:%08.2f"%("Alfred",25,1.70))
#输出效果：
# Name:Alfred     Age:00000025 Height:00001.70
```

### 8.科学计数法
``` py
format(0.0026,'.2e')
#输出效果：
# '2.60e-03'
```