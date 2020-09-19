---
layout: post
title: 共同基礎概念
date: 2020-09-19
author: Brad
tags: [Object detection, DNN]
comments: true
toc: true
pinned: false
---
###### tags: `object detection`
本篇介紹一些物體偵測共同會用到的方法或技巧。
<!-- more -->
## NMS
Non-maximum suppression
> 針對相同物件，從多個候選框中選擇一個正確的方法
![](https://i.imgur.com/ZFDLt33.png)

1. 設定閾值 t
2. 選擇候選信心值最高的 c
3. 將c與其它候選計算IoU(Jaccard Index)
4. 將每個候選超過t的其信心值設為0
5. 重複2~4

## IOU
IOU 是計算兩個框框重疊程度的指標，兩個框框計算 IOU 是交集面積 / 聯集面積，越大表示重疊程度越高，或是說兩個框框越相似。IOU 也會用在計算損失上，候選框框與正確答案的 IOU $IOU=\frac{B_1\bigcap B_2}{B_1\bigcup B_2}$
![](https://i.imgur.com/UwoCkDF.png)