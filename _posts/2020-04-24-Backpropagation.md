---
layout: post
title: Backpropagation
date: 2020-04-24
Author: Brad
tags: [DNN Basic]
comments: true
toc: true
---



神經網路要更新其權重及偏置值只需要對誤差函數微權重或偏置值，然後由於神經網路層數一深，在計算上往往很複雜，為了簡化，有人提出了以$\delta$為主的反向傳播法。<!-- more -->它定義為：

$$
\delta^l_j\equiv\frac{\partial E}{\partial z^l_j}
$$

其中，$E$為誤差函式，這裡以L2誤差為例子，上標代表神經網路層數$l$，下標為第$l$層的第$j$的神經元，$z$為未經過激活函式的值，經過激活函式為$a$。  
根據結果，我們可以利用以下4個方程式更新網路。

$$
\begin{align}
BP1&: \delta^L=\nabla_a E \odot \sigma'(z^L)\\
BP2&: \delta^l=(\delta^{l+1}w^{l+1})\odot\sigma'(z^l)\\
BP3&: \frac{\partial E}{\partial b^l_j}=\delta^l_j\\
BP4&: \frac{\partial E}{\partial W^l_{jk}}=\delta^l_j a^{l-1}_k
\end{align}
$$

## BP1

針對任意層的誤差定義如BP1，而最後一層可因此得到:

$$
\begin{aligned}
    \delta^L_j=\frac{\partial E}{\partial z^L_j}&=\frac{\partial E}{\partial a^L_j}\frac{\partial a^L_j}{\partial z^L_j}\\
    &=\frac{\partial E}{\partial a^L_j}\sigma'(z^L_j)
\end{aligned}
$$

寫成矩陣即

$$
\delta^L=\nabla_a E \odot \sigma'(z^L)
$$

## BP2
針對非最後一層的誤差可由底下得到：

$$
\begin{aligned}
\delta^l_j&=\frac{\partial E}{\partial z^l_j}\\
&=\sum_k{\frac{\partial E}{\partial z^{l+1}_k} \frac{\partial z^{l+1}_k}{\partial z^l_j}}\\
&=\sum_k{\delta^{l+1}_k \frac{\partial z^{l+1}_k}{\partial z^l_j}}
\end{aligned}
$$

其中針對後項化簡可得：

$$
\frac{\partial z^{l+1}}{\partial z^l_j}=\frac{\partial (w_{kj}\sigma(z^l_j)+...)}{\partial z^l_j}=w^{l+1}_{kj} \sigma'(z^l_j)
$$


整理後可得：

$$
\begin{aligned}
\delta^l_j&=\sum_k{\delta^{l+1}_k w^{l+1}_{kj}\sigma'(z^l_j)}\\
&=(\delta^{l+1} w^{l+1})\odot\sigma'(z^l)
\end{aligned}
$$


## BP3
針對任意偏置值可寫成：

$$
\begin{aligned}
\frac{\partial E}{\partial b^l_j}&=\frac{\partial E}{\partial z^l_j} \overbrace{\frac{\partial z^l_j}{\partial b^l_j}}^\text{1}\\
&=\frac{\partial E}{\partial z^l_j}\\
&=\delta^l_j
\end{aligned}
$$

## BP4

針對任意權重可寫成：

$$
\begin{aligned}
\frac{\partial E}{\partial w^l_{jk}}&=\frac{\partial E}{\partial z^l_j} \overbrace{\frac{\partial z^l_j}{\partial w^l_{jk}}}^{a^{l-1}_k}\\
&=\frac{\partial E}{\partial z^l_j} a^{l-1}_k\\
&=\delta^l_j a^{l-1}_k
\end{aligned}
$$
