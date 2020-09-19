---
layout: post
title: TensorFlow:Gradient Tape
date: 2020-09-04
author: Brad
tags: [DNN, TensorFlow]
comments: true
toc: true
pinned: false
---

在 TensorFlow 框架中，提供了自動微分的功能…
<!-- more -->
只要將操作放入Tape，Variable就會自動記錄。另外也可用watch手動記錄。


```python
# tf provides automatic differentiation mechanism
x = tf.Variable(3.)

with tf.GradientTape(watch_accessed_variables=False) as tape:
    tape.watch(x) # to be tested if performance gets better
    y=x**2

grad = tape.gradient(y,x)

print(grad)
```