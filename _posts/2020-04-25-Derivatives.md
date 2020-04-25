---
layout: post
title: Derivatives
date: 2020-04-25
author: Brad
tags: [Math, DNN Basic]
comments: true
toc: true
pinned: false
---

基礎微分是一門相當簡單的數學，而且也常常被很多演算法使用到，尤其是神經網路，本篇蒐集常見基本微分式子。
<!-- more -->

## 基礎
以下皆是y為函數，對x求導

> $y=x^2\Rightarrow y'=2x$

> $y=a^x\Rightarrow y'=a^x\ln{a}$  
> $ln以e為底；lg以10為底；lb以2為底$

> $y=\log_a{x}\Rightarrow y'=\frac{1}{x\ln{a}} (if\quad a=e, \frac{1}{x})$

## 函數
f及g皆為函數

> $(f+g)'=f'+g'$ (外面微完裡面微)

> $(fg)'=f'g+fg'$ (前微後不微+前不微後微)

> $(\frac{f}{g})'=\frac{f'g-fg'}{g^2}$


## 激活函數微分

### Sigmoid

$$
\sigma (x)=\frac{1}{1+e^{-x}}\\
$$

$$
\begin{aligned}
    \sigma'(x)&=(1+e^{-x})^{-1} (外面微完裡面微)\\
    &=\frac{e^{-x}}{(1+e^{-x})^2}\\
    &=\frac{(1+e^{-x}-1)}{(1+e^{-x})^2}\\
    &=\frac{1+e^{-x}}{(1+e^{-x})^2}-\left(\frac{1}{1+e^{-x}}\right)^2\\
    &=\sigma(1-\sigma)
\end{aligned}
$$

### Relu

$$
ReLu:=max(0,x)
$$


$$
ReLu'=\begin{cases}
    1\quad x\ge 0\\
    0\quad x\le 0
\end{cases}
$$

※LeakyReLU 相似

### Tanh

$$
\tanh{x}=\frac{e^x-e^{-x}}{e^x+e^{-x}}
$$


$$
\begin{aligned}
    \tanh'{x}&=\frac{e^x-e^{-x}}{e^x+e^{-x}} (\frac{f}{g}=\frac{f'g-fg'}{g^2})\\
    &=\frac{(e^x-e^{-x})'(e^x+e^{-x})-(e^x-e^{-x})(e^x+e^{-x})'}{(e^x+e^{-x})^2}\\
    &=\frac{(e^x+e^{-x})(e^x+e^{-x})-(e^x-e^{-x})(e^x-e^{-x})}{(e^x+e^{-x})^2}\\
    &=1-\frac{(e^x-e^{-x})^2}{(e^x+e^{-x})^2}\\
    &=1-\tanh^2(x)
\end{aligned}
$$

## 損失函數微分

### MSE

$$
MSE:=\frac{1}{2}\sum{(y_k-z_k)^2}
$$


$$
\begin{aligned}
\frac{\partial (MSE)}{\partial z_i}&=\frac{1}{2}\sum{2*(y_k-z_k)*\frac{\partial(y_k-o_k)}{\partial z_i}}\\
&=\sum{(y_k-z_k)*-1*\overbrace{\frac{\partial z_k}{\partial z_i}}^\text{當k=i才為1}}\\
&=-1(y_i-z_i)\\
&=(z_i-y_i)
\end{aligned}
$$

### Softmax

$$
p_i=\frac{e^{z_i}}{\sum^K{k=1}e^{z_k}}
$$

微分必需分成$i=j$和$i\ne j$。  
當$i=j$時：

$$
\begin{aligned}
\frac{\partial (Softmax)}{\partial z_j}&=\frac{e^{z_i}\sum_k{e^{z_k}-e^{z_j}e^{z_i}}}{(\sum_k{e^{z_k}})^2}(\frac{f}{g}=\frac{f'g-fg'}{g^2})\\
&=\frac{e^{z_i}(\sum_k{e^{z_k}-e^{z_i}})}{(\sum_k{e^{z_k})^2}}\\
&=\frac{e^{z_j}}{\sum_k{e^{z_k}}}\times\frac{(\sum_k{e^{z_k}-e^{z_j}})}{\sum_k{e^{z_k}}}\\
&=\frac{e^{z_j}}{\sum_k{e^{z_k}}}\times\left(\frac{\sum_k{e^{z_k}}}{\sum_k{e^{z_k}}}+\frac{-e^{z_j}}{\sum_k{e^{z_k}}}\right)\\
&=p_i(1-p_j)
\end{aligned}
$$

當$i\ne j$時：

$$
\begin{aligned}
\frac{\partial (Softmax)}{\partial z_j}&=\frac{0-e^{z_j}e^{z_i}}{(\sum_k{e^{z_k}})^2}\\
&=\frac{-e^{z_j}}{\sum_k{e^{z_k}}}\times\frac{e^{z_i}}{\sum_k{e^{z_k}}}\\
&=-p_j\cdot p_i
\end{aligned}
$$

整理：

$$
\frac{\partial p_i}{\partial z_j}=\begin{cases}
\begin{aligned}
    &p_i(1-p_j)\quad &i=j\\
    &-p_i\cdot p_j \quad &i\ne j
\end{aligned}
\end{cases}
$$


### Cross Entropy微分


$$
CE:=-\sum_k{y_k\log{(p_k)}}
$$

$$
\begin{aligned}
\frac{\partial(CE)}{\partial z_i}&=-\sum_k{y_k\frac{\partial \log{(p_k)}}{\partial p_k}\times\frac{\partial p_k}{\partial z_i}}\\
&=-\sum_k{y_k\frac{1}{p_k}\times\frac{\partial p_k}{\partial z_i}}
\end{aligned}
$$

如果最後一層使用Softmax，可再推導為：

當$k=i$時：

$$
\require{cancel}
k=i\Rightarrow -y_i\cdot\frac{1}{\cancel{p_i}}\cdot \cancel{p_i}\cdot(1-p_i)=-y_i(1-p_i)
$$

當$k\ne i$時：

$$
\require{cancel}
k\ne j\Rightarrow-\sum_{k\ne j}{y_k\cdot\frac{1}{\cancel{p_k}}\cdot(-p_i\cdot\cancel{p_k})}=p_i\sum_{k\ne j}{y_k}
$$

合併上兩式：

$$
\begin{aligned}
\frac{\partial(CE)}{\partial z_i}&=-y_i(1-p_i)+p_i\sum_{k\ne j}{y_k}\\
&=-y_i+y_ip_i+p_i\sum_{k\ne j}{y_k}\\
&=-y_i+p_i\left(y_i+\sum_{k\ne j}{y_k}\right)
\end{aligned}
$$

因為CE常常搭配one-hot編碼，可得$y_i+\sum_{k\ne i}{y_k}=1$，可化簡為$\frac{\partial(CE)}{\partial z_i}=p_i-y_i$