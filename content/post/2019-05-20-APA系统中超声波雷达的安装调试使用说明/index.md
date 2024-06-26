---
layout:      post
title:       "APA系统中超声波雷达的安装调试使用说明"
subtitle:    ""
description: " "
date:        2024-05-20T00:26:47+08:00
author:      "Ambrose"
image:       "/img/popo.jpg"
published: true 
tags:        ["APA", "超声波雷达"]
URL: "/2024/05/20/apa/"
categories:  [ 🚘Auto ]
# [ 🚘Auto ] [ 🛰️Tech ] [ 🏕️Life ] [ 🎥Video ]
---

@[TOC]
## 前言：
在传统APA自动泊车系统中，通常使用超声波雷达进行车辆前后辈避障以及侧向车位探测。目前市场上大多数带有自动泊车功能的车辆均配有12个超声波雷达，本文从硬件安装及超声波雷达调试标定两方面对自动泊车超声波雷达的安装调试进行说明
![apa1.png](pic/apa1.png)


## 1 硬件安装
自动泊车配置的超声波雷达一般为两组12个雷达探头。单组6个雷达探头串联，其中第1和第6号雷达为长距LRU雷达，2-4号为短距SRU避障雷达。超声探头均采用Lin通讯或IO口进行通讯。
### 1.1 安装位置选择
如前言示意图所示12个超声波雷达，车头和车尾分别为两组探头。其中车辆左右两侧为长距探头，车头车尾避障雷达为短距雷达。短距雷达均匀分布在车头车尾，垂直于车身前后保险杠。没有俯仰角及左右偏角。四只长距超声波雷达垂直于车身车辆对称轴。超声安装高度位于50-60cm 高度左右视情况而定。由于探头垂直方向FOV过大造成探头探地问题可通过安装后调试标定解决。安装完成后的多路超声FOV示意图如下(备注：APA系统中车位探测雷达一般使用车头左右两个侧向长距雷达基本可完成车位探测功能。在其他方案中，车尾长距LRU基本作为检车位传感器冗余配置或者侧向并线辅助系统传感器配置故下图中并未绘制车尾LRU雷达FOV)：
￼![在这里插入图片描述](pic/apa2.png)

### 1.2 安装细节注意
> 超声波雷达由：探头橡胶圈 探头夹具 探芯 下部橡胶圈 探头壳体 背部PCB板 六部分构成。
![在这里插入图片描述](pic/apa3.png)
￼
> 安装过程中使用定制安装夹具，先将安装夹具与车辆保险杠内的夹具定位标识对齐，然后压融粘结。后将超声探头按照夹具配合卡口推如卡紧即可，注意保险杠外侧探头橡胶圈与保险杠连接处侧向挤压力不得过大，安装时注意探头方向。(注意：探头安装过程中必须严格按照超声厂商指导安装方式安装，一般指定车型量产时需提前交付超声厂商车辆前后保险杠数模结构及一套前后保险杠实物进行安装位置设计评估)
![在这里插入图片描述](pic/apa4.png)
￼
## 2 调试标定
### 2.1 探测距离复核
超声波雷达安装完成之后通过Lin或IO连接成功后一般即可进行使用。
实际使用过程中环境温度，空气湿度，震动均可对超声测距造成误差。在复核超声测距过程中如果测距存在误差需考虑环境变量的影响
### 2.2 调试板调试标定
#### 2.2.1 调试板介绍
超声波雷达调试板
￼![在这里插入图片描述](pic/apa5.png)![
    
](image.png)
#### 2.2.2 参数细节解释
超声波雷达调试板上位机显示为原始模拟信号。模拟信号窗口横轴为周期，纵轴为信号强度。探头发出超声波在遇到障碍物后模拟信号峰值会增大。
￼![在这里插入图片描述](pic/apa6.png)
其中单个周期中收到的超声波强度随着距离的增大，TOF时间变长。收到的回波强度越弱。上图调试上位机中蓝色线增益对收到回波强度进行了补偿。
图中粉色线条为阈值。一共9段对不同tof时间段下收到的回波进行了过滤。只有回波峰值超过阈值才能收到障碍物探测，否则会进行忽略。
#### 2.2.3 调试标定标准
超声波雷达调试标定完成后应该在标准工况下测距准确。合理的增益和阈值可以合理增强回波强度，过滤不需要的超声噪点避免超声探地
![在这里插入图片描述](pic/apa7.png)
>  [超声上位机调试软件]https://download.csdn.net/download/rothschildxiaobo/11189885)
