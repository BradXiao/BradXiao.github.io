---
layout: post
title: Hyperparameter  Optimization/Tuning
date: 2020-10-01
author: Brad
tags: Machine_Learning, Data_Preprocessing
comments: true
toc: true
pinned: false
---

<!-- more -->

# Hyperparameter  Optimization/Tuning


機器學習最困難的部份是為模型找到好的超參數，不好的超參數甚至會直接導致不好的模型效果。

## 傳統的手工搜尋
不管是機器學習中的決策樹等演算法或最新的深度學習，手動調整即利用調整者之實務經驗來給定最佳參數，通常會先初步建模並測試，有了正確率數值再進一步憑感覺調整超參數。
>缺點是沒辦法保證找到最好的組合、非常耗時

## 網格搜尋
假設有兩個待試超參數$P_1, P_2$，其集合為$P_1=\{1,2,3\}, P_2=\{a,b,c\}$，由此兩超參數可以組合出$P_1\times P_2$個組合，再使用CV(交叉驗證技巧)，每個組合可得到一組數值，選最高即為最佳參數。
![](https://i.imgur.com/GfeMWbj.png)

>雖然可以找到最佳解，但是整個過程仍然非常耗時間，在有些模型如神經網路，有些超參數有無限多種組合，這使得過程會耗更久

## 隨機搜尋
隨機搜尋與網格搜尋相似，但是並非所有組合都去測試，而是隨機選擇n個組合測試，並從中選出最佳者。
![](https://i.imgur.com/MMfj4Fs.png)
>同樣不能保證找到最佳組合

## 貝葉斯搜尋
屬於SMBO演算法(Sequential model-based optimization)，其方法可以概略簡化為：
1. Using previously evaluated points X1:n, compute a posterior expectation of what the loss f looks like.
2. Sample the loss f at a new point Xnew, that maximizes some utility of the expectation of f. The utility specifies which regions of the domain of f are optimal to sample from.

```python=
from skopt import BayesSearchCV

import warnings
warnings.filterwarnings("ignore")

# parameter ranges are specified by one of below
from skopt.space import Real, Categorical, Integer

knn = KNeighborsClassifier()
#defining hyper-parameter grid
grid_param = { 'n_neighbors' : list(range(2,11)) , 
              'algorithm' : ['auto','ball_tree','kd_tree','brute'] }

#initializing Bayesian Search
Bayes = BayesSearchCV(knn , grid_param , n_iter=30 , random_state=14)
Bayes.fit(X_train,y_train)

#best parameter combination
Bayes.best_params_

#score achieved with best parameter combination
Bayes.best_score_

#all combinations of hyperparameters
Bayes.cv_results_['params']

#average scores of cross-validation
Bayes.cv_results_['mean_test_score']
```