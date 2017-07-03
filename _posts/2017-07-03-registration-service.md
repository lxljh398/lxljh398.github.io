---
layout: post
title: "把任意exe程序注册成windows系统服务"
date: 2017-07-03 10:06:05
categories: server
tags: server Windows服务 exe
excerpt: 某软件每次开机都需要手动启动，就算添加成开机启动项，在Windows8以后的系统版本中权限的管理更加严格，开机时并不能成功启动软件，因此在网上搜索把exe注册成系统服务的办法，找到一些帖子和经验综合自己亲测，可将代理程序注册成系统服务开机自动启动而不用每次重启系统都要手动启动程序。
author: fatso
---

* content
{:toc}


## 工具
instsrv.exe——来自Windows 2000 Resource Kits的一个小工具 
 
#### 微软官方对该小工具的说明：
Installs and uninstalls executable services and assigns names to them.
显而易见，这个小工具是用以安装和卸载可执行的服务和指派服务名给这些可执行的服务的。
那么怎么去使用呢？这里我们设定要将F:\cpu.exe 以 CPUSrv 的名称显示作为服务的话，我们应当这样子做：
先将instsrv.exe放入任意目录，我们有两种办法来执行这个命令
### CMD法
+ 1、单击『开始』菜单中的【运行】并键入“cmd”（不包括双引号）后单击【确定】按钮

+ 2、在CMD中使用 cd 命令进入 instsrv.exe 所在目录或者直接输入 instsrv.exe 具体路径。比如 instsrv.exe 在F:\Tools目录下的话，我们应该这样子做：
键入 cd f:\tools 后回车进入该目录
键入 instsrv CPUSrv f:\cpu.exe 回车即可
或者也可以
直接键入 f:\tools\instsrv.exe CPUSrv f:\cpu.exe 后回车即可

+ 3、安装了服务，但此时服务并未启动，我们可以使用 Net 命令来启动服务
依旧在CMD中
键入 net start CPUSrv 后回车即可

+ 4、启动了服务，我们还可以设置服务启动类型
依旧在CMD中
键入 sc config CPUSrv start= auto     自动启动方式
键入 sc config CPUSrv start= demand   手动启动方式
键入 sc config CPUSrv start= disabled 已禁止启动方式
 
### GUI法
+ 1、单击『开始』菜单中的【运行】
+ 2、在【运行】文本框中键入
f:\tools\instsrv.exe CPUSrv f:\cpu.exe
后单击【确定】按钮

+ 3、安装了服务，启动服务
单击『开始』菜单中的【运行】并键入“Services.msc”（不包括双引号）后单击【确定】按钮

+ 4、在【服务】中的名为 CPUSrv 的服务上右击即可执行 启动§停止§重新启动 等菜单命令。双击进入即可设置启动类型。

如果我们要删除这个服务，按照上述步骤，我们执行
instsrv.exe CPUSrv REMOVE
即可删除该服务
注：不要用该工具删除系统有关服务！

我们也可以用这个小工具创建一个服务，并设定以某帐户登录启动该服务，命令格式：
instsrv CPUSrv F:\cpu.exe -a your account name -p password

参考：
[怎样把任意exe程序注册成windows系统服务](http://jingyan.baidu.com/article/59703552fee38f8fc107405c.html)