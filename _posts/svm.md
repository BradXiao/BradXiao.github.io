---
layout: post
title: Support Vector Machine
date: 2020-
author: Brad
tags: [Supervised, Linear Classifier]
comments: true
toc: true
pinned: false
---

支持向量機`Support Vector Machine`是由Cortes和Vapnik於1995年提出，它使用一演算法找出兩器中的分隔線(超平面)，並將與資料點距離(margin)最大化。  
典型SVM只能處理二分類問題，多類可使用one against all/one技巧達成。
<!-- more -->

## SVM最佳化公式
依前段述敘，我們可以
> 定義超平面

$$
w^Tx+b=0
$$

> 定義任意資料點

$$
\forall i, \begin{cases}
w^Tx_i+b\ge 1 \quad \text{if }y_i=1\\
w^Tx_i+b\le 1 \quad \text{if }y_i=1\\
\end{cases}
$$

> 任意點到平面距離

$$
r=\frac{\vert w^Tx+b\vert}{\Vert w\Vert}=\frac{2}{\Vert w\Vert}
$$

> 最佳化問題

$$
\begin{aligned}
&\underset{w,b}{\operatorname{max}} \frac{2}{\Vert w\Vert}, \quad &s.t.\quad y_i(w^Tx_i+b)\ge 1, i=1,2,3,...,m\\
等價於\quad&\underset{w,b}{\operatorname{min}} \frac{1}{2}\Vert w\Vert^2, \quad &s.t.\quad y_i(w^Tx_i+b)\ge 1, i=1,2,3,...,m
\end{aligned}
$$

## 解最佳化

## SVM 其它技巧



















