---
layout: post
title: Normalization
date: 2020-04-07
author: Brad
tags: Data_Preprocessing
comments: true
toc: true
---


Sometimes the features we gather may be in different units. E.g., the temperature may vary from 0 to 50 and the rainfall may vary from 0 to 2000. Feeding the data directly into neural networks may cause some problems. Especially, when sigmoid function is used. This is because the derivatives of sigmoid function in left and right sides are small, the convergence is slowed down. 
<!-- more -->
## Min-max

###  0~1

For any $x^*$:

$$x^*=\frac{x-x_{min}}{x_{max}-x_{min}}$$

###  -1~1

For any $x^*$:

$$x^*=\frac{x-x_{mean}}{x_{max}-x_{min}}$$

## Z-score

This is known as the relative distance between the average. For any $x^*$:

$$x^*=\frac{x-\mu}{\sigma}$$

In the formula, $\mu$ is the mean value of all $x$ and $\sigma$ is the standard deviation which is $\sqrt{\frac{1}{N}\sum{(x_i-\bar{x})^2}}$.

## Others

$$x^*=\frac{log_{10}x}{log_{10}x_{max}}$$

$$x^*=atan(x)\times \frac{2}{\pi}$$

$$x^*=\frac{1}{2}+\frac{1}{2}sin{\frac{\pi (x-\frac{max-min}{2})}{max-min}}$$
