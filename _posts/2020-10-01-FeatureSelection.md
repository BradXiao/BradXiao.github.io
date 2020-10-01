---
layout: post
title: Feature Selection
date: 2020-10-01
author: Brad
tags: [Machine Learning, Data Preprocessing]
comments: true
toc: true
pinned: false
---



# Feature Selection
###### tags: `Machine Learning` `Data Preprocessing`

特徵選取是為了減少特徵數量、降維，使得模型的泛化能力更強，進而提升模型的性能

<!-- more -->

## Removing features with low variance
針對某個離散的特徵數值比如0和1，在所有資料中有95%都是1，只有5%是0，那麼該特徵可以去掉。

此步驟屬於預處理

## Univariate feature selection
針每某個特徵，利用其與應變量(label)之間的關係所得之數值，來進行選擇

### Pearson Correlation
此為常見的相關係數，細節略，底下為程式碼。
**需要注意的是，此係數指能對線性關係敏感，對於非線性變化數值不得以參考**

```python=
import numpy as np
from scipy.stats import pearsonr

np.random.seed(0)
size = 300
x = np.random.normal(0, 1, size)
print "Lower noise", pearsonr(x, x + np.random.normal(0, 1, size))
print "Higher noise", pearsonr(x, x + np.random.normal(0, 10, size))
```

### Mutual information and maximal information coefficient (MIC)
此數值可用來衡量兩個變數之間的線性、非線性關係。

#### Mutual information
為訊息論裡一種有用的訊息度量，可以看作是一個密機變數中包含另一個隨機變數的訊息量

```python=
from minepy import MINE

m = MINE()
x = np.random.uniform(-1, 1, 10000)
m.compute_score(x, x**2)
print m.mic()
```

### Distance correlation
此係數為了克服Pearson而生的，能更好抓取非線性關係。

### Model based ranking
直接使用機器學習演算法針為每個特徵建模

## 線性模型和正則化

### 線性模型
線性模型其運作原理將每個特徵予以一權重並進行分類，該權重某種程度可以視為特徵的重要程度

### 正則化
L1/L2正則化有特徵選取的功效，藉由模型訓練的期間，使得比較不重要的特徵權重變小。L1/L2正則化有時候也稱Lasso和Ridge。

#### L1
$Loss'=Loss+\alpha ||w||_1$
透過一係數$\alpha$來使得不重要特徵權重為零，達到特徵篩選的效用。

#### L2
$Loss'=Loss+\alpha ||w||_2$
L2使得每個特徵之權數趨於相同，相對於L1效為穩定。
```python=
from sklearn.linear_model import Ridge
from sklearn.metrics import r2_score
size = 100

#We run the method 10 times with different random seeds
for i in range(10):
    print "Random seed %s" % i
    np.random.seed(seed=i)
    X_seed = np.random.normal(0, 1, size)
    X1 = X_seed + np.random.normal(0, .1, size)
    X2 = X_seed + np.random.normal(0, .1, size)
    X3 = X_seed + np.random.normal(0, .1, size)
    Y = X1 + X2 + X3 + np.random.normal(0, 1, size)
    X = np.array([X1, X2, X3]).T


    lr = LinearRegression()
    lr.fit(X,Y)
    print "Linear model:", pretty_print_linear(lr.coef_)

    ridge = Ridge(alpha=10)
    ridge.fit(X,Y)
    print "Ridge model:", pretty_print_linear(ridge.coef_)
```

## 隨機森林
此類演算法是以某種計算數值的方式來達成決策樹的建立，而計算數值方式某種程度上也是特徵重要性的計算

### Mean decrease impurity
```python=
from sklearn.datasets import load_boston
from sklearn.ensemble import RandomForestRegressor
import numpy as np

#Load boston housing dataset as an example
boston = load_boston()
X = boston["data"]
Y = boston["target"]
names = boston["feature_names"]
rf = RandomForestRegressor()
rf.fit(X, Y)
print "Features sorted by their score:"
print sorted(zip(map(lambda x: round(x, 4), rf.feature_importances_), names), 
             reverse=True)
```

### Mean decrease accuracy
另一種方法為將同一個特徵內的數值隨機打亂。對於不重要的特徵，打亂對正確率影響較小。
```python=
from sklearn.cross_validation import ShuffleSplit
from sklearn.metrics import r2_score
from collections import defaultdict

X = boston["data"]
Y = boston["target"]

rf = RandomForestRegressor()
scores = defaultdict(list)

#crossvalidate the scores on a number of different random splits of the data
for train_idx, test_idx in ShuffleSplit(len(X), 100, .3):
    X_train, X_test = X[train_idx], X[test_idx]
    Y_train, Y_test = Y[train_idx], Y[test_idx]
    r = rf.fit(X_train, Y_train)
    acc = r2_score(Y_test, rf.predict(X_test))
    for i in range(X.shape[1]):
        X_t = X_test.copy()
        np.random.shuffle(X_t[:, i])
        shuff_acc = r2_score(Y_test, rf.predict(X_t))
        scores[names[i]].append((acc-shuff_acc)/acc)
print "Features sorted by their score:"
print sorted([(round(np.mean(score), 4), feat) for
              feat, score in scores.items()], reverse=True)
```
