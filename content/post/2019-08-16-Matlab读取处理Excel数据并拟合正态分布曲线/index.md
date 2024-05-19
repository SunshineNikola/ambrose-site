---
layout:      post
title:       "Matlab读取处理Excel数据并拟合正态分布曲线"
subtitle:    ""
description: " "
date:        2024-05-20T00:46:35+08:00
author:      "Ambrose"
image:       "/img/popo.jpg"
published: true 
tags:        ["Matlab", "正太分布"]
URL: "/2024/05/20/nihe/"
categories:  [ 🛰️Tech ]
# [ 🚘Auto ] [ 🛰️Tech ] [ 🏕️Life ] [ 🎥Video ]
---

clc,clear,close;
% 导入数据
data = xlsread('5555.xlsx',1);
RealWidth = data(:,2);
DetectWidth = data(:,1);
D_Value = DetectWidth - RealWidth;

% 求 D_Value 平均值

% 绘制散点图像
subplot(2,1,1);
plot(D_Value,'*');
title('垂直库位停车后相对位置误差离散图');
ylabel('库位检出误差');
% set(gca,'YTick',[-4:0.2:2]);

% 绘制正态分布直方图
subplot(2,1,2);
histfit(D_Value,60);
title('离散直方图');
xlabel('误差值');
% ylabel('数量'); 
% set(gca,'XTick',[-4:0.2:2]);

% 拟合曲线
% f = normpdf(x,均值,标准差)

