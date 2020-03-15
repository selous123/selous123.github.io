---
title: 光流法(optical flow)简介
date:  2020-03-14 22:34:13 +0800
category: computer-vision
tags: deep-learning optical-flow
excerpt: 光流预测
comment: True
mathjax: True
---

光流定义：

假设一个目标像素在 $t$ 时刻亮度为 $I(x,y,t)$，在$t+\delta t$时刻，运动$[\delta x, \delta y]$后与 $t$ 时刻具有相同的亮度，即 $I(x,y,t) = I(x + \delta x, y + \delta y, t + \delta t)$，对该约束用泰勒公式进行一阶展开并关于 $t$ 求偏导可以得到光流方程，

$$
I(x,y,t) = I(x,y,t)+\frac{\delta I}{\delta x} dx + \frac{\delta I}{\delta y}dy + \frac{\delta I}{\delta t}dt + \epsilon
$$

假设忽略二阶无穷小 $\epsilon$ 则：

$$\frac{\delta I}{\delta x} dx + \frac{\delta I}{\delta y}dy + \frac{\delta I}{\delta t}dt = 0$$

同时除以 $dt$， 则
$$\frac{\delta I}{\delta x} \frac{dx}{dt} + \frac{\delta I}{\delta y}\frac{dy}{dt} + \frac{\delta I}{\delta t} = 0$$

令 $u = \frac{\delta x}{\delta t}$, $v = \frac{\delta y}{\delta t}$：

$$f_x * u + f_y * v + f_t = 0$$

于是

$$-f_t = f_x * u + f_y * v = \nabla f * \bm{v}$$

也就是：**相同位置的时间灰度微分是空间灰度微分与这个位置上相对于观察者的速度的乘积。**