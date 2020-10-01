---
layout: post
title: Yolo Series
date: 2020-10-01
author: Brad
tags: Object_Detection, DNN
comments: true
toc: true
pinned: false
---

# YOLO Series
You Only Look Once
為end-to-end單一網路設計，速度上相較於**典型**的R-CNN快
特色為有取box的方法，網路為一般CNN網路架構
與其它的方法差異是YOLO分類時是有考慮整張圖的

<!-- more -->

> [來源](https://towardsdatascience.com/guide-to-car-detection-using-yolo-48caac8e4ded)
![](https://i.imgur.com/a2Au4zL.png)

## 定義
* 框定 S*S 均勻的grids (每個grid負責 gound truth box 中心落在其上的)
* 每個grid預測B個bounding box和一個有C類的類別值(不是每個bounding box都有)
    每個bounding box有x,y,w,h和信心值
    信心值= $P_r(Object)\times IOU^{truth}_{pred}$

## 訓練
* YOLO其神經網路為典型的CNN，並沒有特別設計
* 比較特別的是輸出
    輸出之張量為 $S\times S\times (B\times 5+C)$
    原論文之S=7, B=5, C=20
    
另外loss設計也藏著許多調整，詳細可參照論文

## 使用
針對神經網路輸出會有多個候選框，這些框先做初步篩選後(擼掉類別期望值小於<font color="red">**門檻**</font>)，再套用NMS
![](https://i.imgur.com/AJ7jHOg.png)
 

左圖看起來有經過類別預過濾，右圖是NMS執行之後，可以看出差異

![](https://i.imgur.com/fBKZnA5.png)

**[程式碼: 如網頁所示](https://towardsdatascience.com/guide-to-car-detection-using-yolo-48caac8e4ded)**

# 其它變種
## YOLO v2 (YOLO 9000)
最大的改變是將最後一層的FC改為卷積層，其它為細節的修改
輸出維度  $S\times S\times\times B\times (5+C)$ (定義同YOLO v1)
* 使用 Batch Normalization
* 增加輸入影像解析度
* 套用anchor box
原本bounding box是由網路直接預測位置，anchor box即是由事先定義的位置，網路改為預測其相對位置(偏移量)
    * (dimension clusters)anchor box原為先定義，改為由訓練使用k-means得到
        * k-means 距離有經過特別設計，詳參考論文
        ![](https://i.imgur.com/6Euvn3C.jpg)

* (direct location)預測與anchor box的偏移量時，加入了限制，減少訓練難度
    * 原本預測位置的數值沒有限制(任意)，改為加入sigmoid，限制其為0~1
    ![](https://i.imgur.com/QuRADfb.jpg)

* (fine-grained features)增加passthrough層，改善小物體檢測
    * 經過sampling之特徵對小物體不利，利用residual概念改善
    ![](https://i.imgur.com/9WMnuGe.jpg)

* (multi-scale training)訓練時，動態輸入不同大小的影像
* (joint classification and dectection)訓練先訓練標bounding box的能力，之後再同時訓練標bounding box及分類的能力
* (dataset combination with wordtree) 將預測類別建樹，同階的予以一個softmax單位
[Details](https://zhuanlan.zhihu.com/p/47575929)
    ![](https://i.imgur.com/ZDfnoyj.jpg)

## YOLO v3 
* bounding box改為動態
* 改善分類網路
* 不使用v2的softmax方式，而由每類的logistic代替


## YOLO v4 
基於YOLO v3上，使用大量的改進及技巧
YOLOv4 = CSPDarknet53+SPP+PAN+YOLOv3

* Weighted-Residual-Connections (WRC)
* Cross-Stage-Partial-connections (CSP)
* Cross mini-Batch Normalization (CmBN)
* Self-adversarial-training (SAT)
* Mish-activation
* Mosaic data augmentation
* CmBN
* DropBlock regularization
* CIoU loss
* CSPResNeXt50 / CSPDarknet53
* EfficientNet-lite / MixNet / GhostNet / MobileNetV3


## YOLO v5 
目前尚未被官方承認，模型也持續改進中(2020.09.18)
相對於YOLO v4沒有大幅度的更動



---
## 其它
### Non Maximum Suppression
參照共同基礎概念

### IOU
參照共同基礎概念



