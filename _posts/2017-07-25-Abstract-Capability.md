---
layout: post
title: "抽象能力培养"
date: 2017-07-25 15:36:05
categories: 能力提升
tags: 个人
excerpt: 抽象能力是程序员最关键的能力，是区分优秀程序员和普通程序员的最重要指标。
author: fatso
---

* content
{:toc}

摘要: 抽象能力是程序员最关键的能力，是区分优秀程序员和普通程序员的最重要指标。

激光与金鱼有什么共同之处？

——它们都不会唱歌。

“横看成岭侧成峰”，苏轼的这句诗是对“抽象”的最好隐喻。对同一个事物，你看待它的视角不同，就会呈现不同的面貌。每一个视角，代表该事物某一个维度的抽象。如果你觉得这个“抽象”的概念很“抽象”，我以汽车为例子来进行说明。

一辆汽车，在汽车厂里制造出来时，在厂家眼中它是“产品”，与其他的制造业产品，例如香皂、毛巾、充气娃娃属于同一类，他们关心它的成本、出产日期、制造平台、型号等属性。在装船发运给经销商的时候，在船运公司眼中它是“货物”，与同船运输的衣服、微波炉、手机属于同一类，他们关心它的重量、体积等属性。当它送到经销商的商店里之后，在经销商的眼中它是“商品”，与店中同时售卖的润滑油、配件和工具属于同一类，他们关心它的生产厂家、价格等属性。当汽车卖出之后，在车主眼中它是“交通工具”，与飞机、轮船、自行车属于同一类，他们关心它的速度、油耗、安全性、舒适度等属性。假如车主不幸遭遇破产清算，在清算人员的眼中它是“财产”，与房子、银行账户、家具属于同一类，他们关心它的价值、折旧率等属性……

上面的一段论述中，一辆“汽车”是一个具体的事物，而“产品”、“货物”、“商品”、“交通工具”、“财产”则是它各个维度的抽象。横看是产品，侧看是货物，“上看下看左看右看”分别是不同的抽象类型。

抽象是对事物的简化，通过只关注事物的一个维度而忽略其他所有方面，有效地降低了复杂性。例如上面汽车的例子中，作为船运公司，他们只关心汽车重量和体积等属性，而把它的最大速度、油耗、安全性、舒适性等属性完全忽略。这样就可以大大降低“概念重量”，即需要同时理解、记忆和处理的知识量。Grady Booch（UML三友之一）在他的《Object-Oriented Analysis and Design with Applications》一书中说：“一颗垂死的恒星正处在塌缩的边缘，一名儿童正在学习如何阅读，白细胞向病毒发起进攻——这是真实事件的几个例子，它们包含着真正可怕的复杂性。”没有抽象，我们根本无法处理这么巨大的复杂性。

即使我们需要同时处理事物的多个维度，也有必要把它们按维度划分为不同的抽象，每次处理一个维度。在建筑装修行业中，装修设计师会针对房屋分别绘制一份电路布线图和一份给排水管网图，而不是在同一张图上同时绘制电路布线和供水管网。

事实上，我们一直生活在抽象的世界，抽象是我们认识世界、描述世界和改造世界的基本工具。没有抽象，我们马上降低到其他动物的层次。我们每天随口说的一句话里面都包含几个抽象。很多抽象已经具体到我们忘记了它是一个抽象了。例如“国家”、“足球队”、“胜利”、“努力”，哪一个不是抽象？蚂蚁的眼中有“中国”和“美国”吗？

对于软件开发来说，问题域和解决方案域中都充满了抽象。问题域中的各种名词、动词、形容词，基本上都是一个个抽象。好的抽象能够应用到非常宽广的领域。例如“大”这个形容词抽象，最早也许只是用来比较体积，后来你会发现它也可以用来比较高度、深度、长度，再后来甚至可用来比较两个人成就的大小、官位的大小、胸怀的大小、发展前景的大小……。在Java中，已经通过接口Comparable表达了这个抽象，实现了Comparable接口的类型可以通过compareTo()方法比较大小。这样我们就可以知道：博士比学士大，教授比讲师大，等等等等。

综上所述，抽象有两个作用，一个是消除不必要的复杂性（“研究颜色时不管重量和体积”），另一个是发现共性（从货物的角度看，汽车和衣服是同一类东西）。

在问题域中发现本质的抽象并面向抽象编程，是创建高质量软件、保证软件简单性、一致性、灵活性和扩展性对立统一的主要手段。