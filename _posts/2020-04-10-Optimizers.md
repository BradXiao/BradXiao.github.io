---
layout: post
title: Optimizers
date: 2020-04-10
Author: Brad
tags: [DNN Basic]
comments: true
toc: true
---

# Gradient Descent

對一向量偏微分可取得該變數之最大變化之方向和量，稱為梯度。通常函數為誤差函數，越低越好，故要減去梯度，稱梯度下降法。在某些場和需要梯度上升法，如函數為Rewards函數。<!-- more -->典型的神經網路更新方法為：

$$
W\leftarrow W-\eta \frac{\partial L}{\partial W}
$$

※ 令梯度等於零即最大/最小值

## Momentum
由物理動量概念而來，其表示當梯度大的時候，其動量較大，比較不會卡住(local optima)。其定義：

$$
V_t\leftarrow \beta V_{t-1}-\eta\frac{\partial L}{\partial W}\\
W\leftarrow W+V_t
$$

# AdaGrad
動態調整學習速率，當前面梯度較小，放大學習速率；當前面梯度較大，縮小學習速率

$$
\begin{aligned}
W&\leftarrow W- \eta\frac{1}{\sqrt{n+\epsilon}}\frac{\partial L}{\partial W}\\
&n=\sum_{t=1}^r{(\frac{\partial L_t}{\partial W_t})^2}\\
\end{aligned}
$$

# RMSprop
在AdaGrad中，訓練到後來，其$n$會越來越大，造成訓練變極慢，故改進n，將之前梯度縮小影響範圍

$$
n=\rho(\sum_{t=1}^{r-1}{(\frac{\partial L_t}{\partial W_t})^2})
+(1-\rho)\frac{\partial L_r}{\partial W_r}
$$

# Adam

結合 Momentum 和 RMSprop，其中$\hat{m_t}$和$\hat{v_t}$之分母為bias correction，即一開始時$m=0, v=0$，能校正為$\require{cancel}\hat{m_1}=\frac{\cancel{\beta 0}+\cancel{(1-\beta)}\frac{\partial L}{\partial W}}{\cancel{1-\beta}}$。

$$
\begin{aligned}
W&\leftarrow W-\eta\frac{1}{\sqrt{\hat{v_t}}+\epsilon}\hat{m_t}\\
\hat{m_t}&=\frac{m_t}{1-\beta^t_1}\\
\hat{v_t}&=\frac{v_t}{1-\beta^t_2}\\
m_t&=\beta_1m_{t-1}+(1-\beta_1)\frac{\partial L_t}{\partial W_t}\\
v_t&=\beta_1v_{t-1}+(1-\beta_2)(\frac{\partial L_t}{\partial W_t})^2
\end{aligned}
$$