---
layout: post
title: Pearson Corelation Coefficient
date: 2020-04-09
author: Brad
tags: [Math]
comments: true
toc: false
pinned: false
---

A coefficient is used to measure the corelation of two groups of variables. It only measures the linear relation. The value is between -1~1. Assume there are two sets $X$ and $Y$. There are $n$ samples in each. We can get:
<!-- more -->

$$
r=\frac{S_{XY}}{S_XS_Y}=\frac{\frac{\sum(x_i-\bar{x})(y_i-\bar{y})}{n}}{\sqrt{\frac{\sum(x_i-\bar{x})^2}{n}}\sqrt{\frac{\sum(y_i-\bar{y})^2}{n}}}
$$