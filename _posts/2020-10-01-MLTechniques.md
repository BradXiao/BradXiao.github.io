---
layout: post
title: Machine Learning Techniques
date: 2020-10-01
author: Brad
tags: Machine_Learning Data_Preprocessing
comments: true
toc: true
pinned: false
---


# ML Techniques

解讀對的資訊加上對的策略能幫助你省掉很多試參數的時間

## 資料的設置
傳統的資料分類方法是將所有資料分成訓練70%測試30%，這會發生一種情況，即你用網路爬下的資料來訓練及測試，結果實際使用正確率很低，原因是實際使用和網路爬下的資料分布不太相同，所以更合理的做法如下：

<!-- more -->

### 資料的劃分
* 訓練集：你的訓練資料，用來訓練模型
* 開發集(development)：用於調整超參數
* 測試集(test)：評估模型，不作任何調整
需注意的是，與傳統的不同，**開發與測試集之分布應該盡可能與實際使用的資料相吻合，而且開發與測試集的分布要盡可能相同**

`非常重要的一點，不要使用測試集來調整模型參數(權重,超參數,演算法等)，否則會失去generalize能力`

### 模型評估：使用不同的標準
常見的有Precision, Recall, F1 Score, etc.
`針對少數情況，比如重視誤判率需要特別選擇評估指標`

##### 多項不同性質數據比較
當有其它指標需要評估時(比如執行時間、模型參數量)，可以設定在相同基準點下，最大化模型效果。
比如在 100ms 內最好正確率的模型

## 誤差分析
快速地有初步計畫並測試比想出完美的模型更好，有可能一開始模型非常非常差，但是先有了初步結果再快速迭代是比較合理的

### 誤差分析
在錯誤的樣本中，先估算分類各個錯誤比例，再針對最大的比例進行調整，否則會造成花很多時間只提升0.5%，即要選擇CP值最高的優先順序調整

### 分析表格
在分析錯誤時，可以以類似表格形式記錄並計算比例，以選擇最大比例為優先
![](https://i.imgur.com/1rb8RXy.png)
比如有IG濾鏡的會造成錯誤率上升，這時可考慮要不要訓練一個除濾鏡的網路

### 拆分開發集
將開發集拆分成兩個子集(90%,10%)，任兩集合分布一定要相同
針對10%資料集命名為eyeball或ear，用於錯誤分析、製髼分析表格
針對90%命名為blackbox，用於調參
`當eyball或ear集發生過擬合時，應重新抽，重複此循環`

#### eyeball集大小
在此集合中，其中錯誤樣本應至少佔500以下，越多越好，否則不能很好地觀察錯誤情況
若錯誤樣本太少，應擴大eyeball集
* 如果整個開發集太小，有時候可能要全部充當eyeball集
* 如果整個開發集錯誤樣本非常少或樣本本身人類也無法很好地表現，請使用後緒其它技巧

## 偏差和方差
偏差(bias)指的是訓練集的錯誤率；方差(variance)指的是開發集或測試集距離訓練的錯誤率差異

### 處理方式
* 高偏差
    * 加大網路
        * 加大網路通常意味著容易過擬合，要適度使用正則化方法
    * 根據誤差分析修改/新增/減少特徵
    * 減少正則化 (有過擬合風險)
    * 修改模型架構
    * ~~加入訓練資料~~ (可能沒用)
* 高方差
    * 加入訓練資料
    * 加入正則化
    * 使用early stop
    * 特徵處理
        * 增加/減少特徵
        * 特別處理如使用某項算法、公式、轉換
    * 減少網路(不推薦)
    * 根據誤差分析修改/新增/減少特徵
    * 修改模型架構

## 學習曲線
由不同的樣本所畫出的學習曲線(不同於一般x軸是訓練代數)
![](https://i.imgur.com/NGydIHI.png)

### 高偏差
此時增加訓練資料集並沒有效果，這個情況有可能是我們錯估了desired performance
![](https://i.imgur.com/7m9pGHZ.png)

### 抽取數據
注意抽的時候，每個類分之樣本數量所形成之分布，必需要與所有資料相同。
比如有8/10負樣本，2/10正樣本，那抽出的資料必需維持相同比例


## 與人類表現相比
當機器還沒超過人類水平，此時進展會很快
當機器已超越人類水平，此時進展就會非常慢
### 不同任務的難度
* 識別貓或狗
    * 標記容易、容易誤差分析、期望錯誤率容易估計
* 推薦一本書
    * 不容易標記、難以誤差分析
* 預測股票走向
    * 不容易分析、難以改進、難以決定期望錯率

### 定義人類水平
* 分清楚業餘、專家、專家團隊的水平
    * 以最高標準為目標

## 不同分布的訓練和測試
假設我們有1000張使用者上傳的影像，另外從網路上抓取了5000張。對於直覺的訓練、開發、測試集的劃分，通常是全部混合並打亂，再按比例分，但是本書**不推薦這種做法**。實際理想的應遵從：
>開發集和測試集要能反映實際應用場合的分布
按上面的例子，一個可能的劃分方式為5000張網路影像+500張使用者上傳當作訓練，剩下500張分為開發與測試集

### 從網路另外蒐集更多訓練資料的好與壞
傳統人工設計的模型，加入不同分布的資料可能使得模型效果更差，但是現在神經網路反而有更多資料可能效果會更好、也可能會更壞，如下
1. 新的不同分布之資料提供了神經網路在原分布以外的知識
2. 它強迫神經網路的某部份來學習不同資料分布的特性(如不同的解析度、不同的背景等)，如果兩分布差異甚大，可能會導致神經網路在實際效果變差
第二點用白話說，即是無用的事實、知識在有限的空間中把有用的事實、知識擠了出去，這可能可以藉由增加網路容量來克服
> 假如今天要做貓的偵測，資料中有許多任意圖片如人員、地點、地標、動物等，但是其中另涵蓋了大量的文件檔案，顯然這些文件與其它圖片分布差異甚大，它們應該被排除

### 給數據加權重
對於不同分布的資料集，有時候可以透過加權重的方式來達到公平的對待。比如有來自使用者的圖片和網路圖片1:40，我們可以透過令$\beta=\frac{1}{40}$來使得訓練是公平的

$min_\theta\sum_{User}{loss}+\beta\sum_{Internet}{loss}$


### 數據不匹配
假設有一個任務，它的表現為：
* 1%的訓練誤差
* 1.5%與訓練集相同分布但沒訓練過的數據誤差
* 10%的開發集誤差

這種情況下即發生的**數據不匹配**，我們可以透過再從訓練集分出一小部份來測試並觀察，如下：
* 訓練集：模型學習的數據(可能有不同分布，假設A+B)
* 訓練開發集：與訓練集相同分布，不拿來訓練
* 開發集：與實際應用分布相同，用來調參
* 測試集：與測試集分布相同，不調參

如果模型在訓練集訓練後，在訓練開發集上表現良好，但是在開發及測試集不好，說明發生數據不匹配，底下建議：
1. 試著理解不同分布的差異
2. 試著找到更多的開發集據
3. 人工合成
比如你正在進行語音辨識，訓練集都是在安靜地方錄的，測試集為噪音下錄的，你剛好有噪音的資料和很多安靜的資料，現在你可以將它們合成。
合成數據需要注意的是，如果你的噪音來自特定的分布而非完整的分布，有可能會使演算法過擬合這些特定情況；也有可能模型會特別學出這些合成的資料，這需要仔細分析。


## 推理演算法

