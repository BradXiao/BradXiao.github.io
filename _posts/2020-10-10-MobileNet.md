---
layout: post
title: MobileNet
date: 2020-10-10
author: Brad
tags:  CNN_Architecture
comments: true
toc: true
pinned: false
---


2015年，ResNet透過residual learning方式使得梯度消失及爆炸得以緩解，然而，隨著此問題解決，但是參數量仍然很大，在嵌入式系統或手機上就有缺點。

而Efficient CNN就是我們要的，它可以更小更快同時準確度在一定水準以上。


<!-- more -->

## MobileNet V1
透過Depthwise Separable Convolution將傳統Convolutio拆成兩個步驟，能達到節省計算量，又不失效果太多：

1. Depthwise convolution 
2. 1x1 convolution(pointwise convolution) 



![](https://i.imgur.com/UJhuDLh.png)

### 參數量差異
![](https://i.imgur.com/r9nhDDc.png)
![](https://i.imgur.com/NRDEsjZ.png)

### 卷積層
![](https://i.imgur.com/bZjjEKM.png)

$$
ReLU6=min(max(0,x),6)
$$


### 網路架構
![](https://i.imgur.com/FOH2him.png)


## MobileNet V2
在V1中，經過分析後發現有些卷積是空的，如下
![](https://i.imgur.com/Sgza80h.png)

經過實驗後推測，對低維度做ReLU容易損失訊息，高維度較不會損失
![](https://i.imgur.com/K6PMt2t.png)

### Linear bottleneck
將原本Pointwise擴張的部份換成投射到較低的維度，並且將激活函式由ReLU6改為Linear
![](https://i.imgur.com/ja5VtXS.png)


### Expansion layer
v1中的深度卷積沒有改變通道能力，可以藉由Pointwise在進入前進行一次，以擴張維度
![](https://i.imgur.com/CfuqGTi.png)
V1V2差異
![](https://i.imgur.com/vJ6rURx.png)

### Inverted residuls
參考resnet加入residual

![](https://i.imgur.com/FZgof16.png)

## MobileNet V3

### 網路基於NAD -> MnasNet
MnasNet是由Google透過強化學習的方式(reward時間、準確度)來搜尋較佳的神經網路架構之一

### 引入v1深度可分離卷積
如MobileNet v1

### 引入v2linear bottleneck
如MobileNet v2

### 引入squeeze and excitation的注意力模型SE
todo

### 新激活函數h-swish(x)
* swish
![](https://i.imgur.com/Cb9R6sx.png)
該函數有無上下界、平滑等特性，且其效果優於ReLU
* h-swish
近似swish之函數
![](https://i.imgur.com/gHkddAI.png)

![](https://i.imgur.com/7TQKbSx.png)


### 結合NAS(platform-aware NAS)和NetAdapt
todo