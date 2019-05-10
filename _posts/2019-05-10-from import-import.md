---
layout: post
title: "Python的from import和import的区别"
date: 2017-06-30 14:06:05
categories: Python
tags: Python-100Days Basis
excerpt: 在Python环境中经常会引入一些包，使用到import +包名，我们在一些代码中也会经常看到 from import +包名的使用，对于初学者就稍微有点不太理解了，我们就在这里简单的对这两种引用包方式做一介绍。
author: fatso
---

* content
{:toc}

## 分析
    开始学习的过程中也不太理解怎么用的，有什么区别，经常还会遇到同一个包在不同的文献当中出现不一样的效果或者报错，比如 “AttributeError: type object 'datetime.time' has no attribute 'localtime'”，这个时候就比较不懵了，在同样的Python环境下，为什么会出现找不到定义对象的问题，然后在知乎上看到了一个回答，通俗易懂，马上就顿悟了

    from import : 从车里把矿泉水拿出来,给我 
    import : 把车给我 
    两个的区别是：from直接指定了具体要的对象，而使用import就是把包括这个对象堆载一起的物体（车）打包带走，这里面可能还包括玩具、帐篷、餐具等等对象，整包带走的缺点就是庞杂，体积大，优点就是要啥有啥，对于初学者来说直接打包带走更好，避免出现让自己不了解的问题，随着内部结构的了解，慢慢的转变成要啥拿啥的转变

## 案例
    用datetime包做个简单的对比：

+ 引入整个datetime包
``` py
import datetime
print(datetime.datetime.now())
```

+ 只引入datetime包里的datetime类
``` py
from datetime import datetime
print(datetime.now())
```

以上内容都正常打印输出

## 最后

如果我们把上面引用整个datetime包的代码调整一下，使用
``` py
print(datetime.now())
```
来打印输出，这个时候就出现了错误：
``` py
AttributeError: module 'datetime' has no attribute 'now'
```
找不到 datetime的now()，因为now())在datetime包的datetime类中，所以必须要使用
``` py
print(datetime.datetime.now())
```
import之后前者是datetime这个包可见 后者是datetime.datetime这个类可见