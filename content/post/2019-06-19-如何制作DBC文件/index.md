---
layout:      post
title:       "如何制作DBC文件"
subtitle:    "CANdb++ 制作DBC文件"
description: " "
date:        2024-05-19T23:51:02+08:00
author:      "Ambrose"
image:       "/img/popo.jpg"
published: true 
tags:        ["DBC", "CANdb++", "Vector"]
URL: "/2024/05/19/dbc/"
categories:  [ 🚘Auto ]
# [ 🚘Auto ] [ 🛰️Tech ] [ 🏕️Life ] [ 🎥Video ]
---

​
# CANdb++ 制作DBC文件
刚看到55号小白鸭的教程挺好的，贴上地址

[第55号小白鸭的教程](关于DBC文件的创建（DBC文件系列其一）_第55号小白鸭-CSDN博客)

一：安装好CANdb++ 软件；
二：打开CANdb++如图
![opencandb](pic/opencandb.png)
创建一个dbc文件
![creatdbc](pic/creatdbc.png)

选择格式的dbc文件
![choice](pic/choice.png)

自己给dbc文件取一个名字
![name](pic/name.png)

然后保存后自然进入这个界面
![setting](pic/setting.png)

创建ACU和 CCU通信（这两个通信设备具体环境命名）
![tongxun](pic/tongxun.png)
![tongxun2](pic/tong2.png)
![tongxun3](pic/tong3.png)
![tongxun4](pic/tong4.png)

创建一个ID
![chuang1.png](pic/chuang1.png)

根据给定的文档设定下面参数
![set1](pic/set1.png)

设定周期：
![setzhouqi.png](pic/setzhouqi.png)

添加ID：
![jiaxinghao](pic/jiaxinghao.png)


上面设定参数用到文档：

DBC文档的一帧完成了，重复创建ID signal；完成全部文件：

添加ACU 与CCU之间的通信id和信号：双击ACU

添加相应ACU 发送ID

添加CUU请求信号
