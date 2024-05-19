---
layout:      post
title:       "Matlab 绘制三维散点图并进行曲面拟合"
subtitle:    ""
description: " "
date:        2024-05-20T00:44:18+08:00
author:      "Ambrose"
image:       "/img/popo.jpg"
published: true 
tags:        ["Matlab", "散点图"]
URL: "/2024/05/20/lisan/"
categories:  [ 🚘Auto ]
# [ 🚘Auto ] [ 🛰️Tech ] [ 🏕️Life ] [ 🎥Video ]
---

# Matlab散点绘制及曲面拟合
***
* 近期在进行APA自动辅助泊车项目的测试过程中遇到多变量对结果的影响分析问题。需要对自动泊车库位检测系统中车辆行驶速度及距离库位侧向距离对检出准确度 的影响进行分析。进行了简单数据处理及分析
代码如下：
> close all; clc
data = load('C:\Users\temp\Downloads\APA数据处理\APA检车位数据垂直.txt');
speed = data(:,4);
distanceY = data(:,6);
Dvalue = data(:,8);
% 绘制散点图
figure(1),plot3(speed,distanceY,Dvalue,'*');
xlabel('CARSpeed');
ylabel('distanceY');
zlabel('Dvalue');
title('平行库位长度误差与速度及侧向距离相关图')
hold on
% 对散点图进行曲面拟合寻找规律
[X,Y,Z]=griddata(speed,distanceY,Dvalue,linspace(min(speed),max(speed))',linspace(min(distanceY),max(distanceY)),'v4');
figure(2),surf(X,Y,Z);
xlabel('CARSpeed');
ylabel('distanceY');
zlabel('Dvalue');
title('平行库位长度误差与速度及侧向距离关系曲面拟合')


具体结果为下图
![在这里插入图片描述](pic/lisan.png)




