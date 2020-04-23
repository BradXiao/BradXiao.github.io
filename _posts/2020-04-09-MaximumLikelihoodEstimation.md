---
layout: post
title: Maximum Likelihood Estimation
date: 2020-04-09
Author: Brad
tags: [Math]
comments: true
toc: false
pinned: false
---


# Maximum Likelihood Estimation
根據觀察結果來求得每個變數的最大機率。假設我有一袋由$p$比例的白球和$(1-p)$比例的黑球，我今天隨機取一顆球，取完記錄並放回去，以供下一次重取。如此重複100次，故這回合的抽樣結果可表示為：
<!-- more -->

$$
\begin{aligned}
P(樣本結果|模型)&=P(x_1,x_2,...,x_{100}|模型)\\
&=P(x_1|模型)P(x_2|模型)...P(x_{100}|模型)\\
&=p^{70}(1-p)^{30}
\end{aligned}
$$

由上式可知，$p$可以是任意數，不過根據最大似然估計概念，觀察到抽樣結果即是最大機率，故此為一個求一$p$使得$P(樣本結果\mid模型)$ 為最大的最佳化問題。求法為導數等於零。

令抽樣結果為$\theta$，其最大似然估計被定義為：

$$
\begin{aligned}
\theta&=\underset{\theta}{\operatorname{arg max}}P(X; \theta)\\
&=\underset{\theta}{\operatorname{arg max}}\prod P(x; \theta)
\end{aligned}
$$

由於計算數值時，常常會得到一個很小的數，我們常常加上log和取平均，如下：

$$\theta=\underset{\theta}{\operatorname{arg max}} \mathbb{E}[log P(x; \theta)]$$