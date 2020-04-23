---
layout: post
title: Linear Regression
date: 2020-04-05
Author: Brad
tags: [Supervised,Linear Classifier]
comments: true
toc: true
pinned: false
---


# Linear Regression
Here is an example of linear regression using least square method.
<!-- more -->
## Simple Linear Regression

There are $n$ samples and each sample is denoted by $x_i\in[x_1, x_2, ..., x_n]$. For each sample, there is a label(answer) for it denoted by $y_i\in[y_1, y_2, ..., y_n].$ I would like to use a formula to predict every $y_i$ using $x_i$. The predicted $y_i$ is then denoted by $\hat{y}_i$. The formula is then shown below.

$$\hat{y}_i=ax_i+b$$

In order to make correct predictions, $a,b$ are obtained by minimizing a pre-defined loss function.

$$\underset{a,b}{\operatorname{argmin}}Loss(y,\hat{y})$$

A common loss function is mean square error.

$$MSE=\frac{1}{n}\sum_{i=1}^n (y_i-\hat{y}_i)^2$$

### Least Square Method
This is a common method where we find the derivatives of the loss function w.r.t. $a,b$. The derivatives equal to zero. There are two variables $a,b$.  
For b,

$$\frac{\partial Loss(y_i,\hat{y}_i)}{\partial b}=0$$

$$\begin{aligned}
\frac{\partial Loss(y_i,\hat{y}_i)}{\partial b}&=\frac{1}{n}\frac{\partial \sum(y-\hat{y})^2}{\partial b} \\
&=\frac{1}{n}\sum\frac{\partial (y_i-ax_i-b)^2}{\partial b}\\
&=\frac{1}{n}\sum 2(y_i-ax_i-b)(-1)\\
&=-2\frac{1}{n}\sum (y_i-ax_i-b)
\end{aligned}$$
  
Since $\frac{\partial Loss(y_i,\hat{y}_i)}{\partial b}=0$, we can get:  

$$\begin{aligned}
&\Rightarrow -2\frac{1}{n}\sum (y_i-ax_i-b)=0\\
&\Rightarrow b=\sum(y_i-ax_i)
\end{aligned}$$

#### For a,

$$\frac{\partial Loss(y_i,\hat{y}_i)}{\partial a}=0$$

$$\begin{aligned}
\frac{\partial Loss(y_i,\hat{y}_i)}{\partial a}&=\frac{1}{n}\frac{\partial \sum(y-\hat{y})^2}{\partial a} \\
&=\frac{1}{n}\sum\frac{\partial (y_i-ax_i-b)^2}{\partial a}\\
&=\frac{1}{n}\sum 2(y_i-ax_i-b)(-x_i)\\
&=-2\frac{1}{n}\sum (y_i-ax_i-b)x_i
\end{aligned}$$
  
Since $\frac{\partial Loss(y_i,\hat{y}_i)}{\partial b}=0$, we can get:  

$$\begin{aligned}
&\Rightarrow -2\frac{1}{n}\sum (y_i-ax_i-b)x_i=0\\
&\Rightarrow \sum (y_i-ax_i-b)x_i=0\\
&\Rightarrow a=\frac{\sum (y_ix_i-bx_i)}{\sum x_i^2}
\end{aligned}$$


## Multiple Regression

For multiple regression, the only difference is $b$ is included as part of $A$ and additional 1 column is included as part of $X$.

$$\begin{aligned}
Y&=XA\\
[n\times1]&=[n\times(d+1)][(d+1)\times 1]\\
    \begin{bmatrix}
    y_1 \\ y_2 \\ \vdots \\ y_n
    \end{bmatrix}
    &=
    \begin{bmatrix}
    1,[X_1]\\ 1,[X_2] \\ \vdots \\ 1,[X_n]
    \end{bmatrix}
    +
    \begin{bmatrix}
    b \\ A_1 \\ \vdots \\ A_d
    \end{bmatrix}
\end{aligned}$$

As we saw in simple regression, the loss function is $(y-\hat{y})^2$. Loss function for matrix is:

$$\begin{aligned}
Loss(A)&=(Y-\hat{Y})^T(Y-\hat{Y})\\
&=Y^TY-y^T\hat{Y}-\hat{Y}^TY+\hat{Y}^T\hat{Y}\\
&=Y^TY-y^TXA-A^TX^TY+A^TX^TXA\\
&(Y^TXA)^T=A^TX^TY\\
&=Y^TY-Y^TXA-(Y^TXA)^T+A^TX^TXA\\
&Y_TXA=A^TX^TY (1\times 1 \; matrix)\\
&=Y^TY-A^TX^TY-A^TX^TY+A^TX^TXA\\
&=Y^TY-2A^TX^TY+A^TX^TXA
\end{aligned}$$

Now the derivatives of loss function w.r.t. $A$ is:

$$\begin{aligned}
\frac{\partial Loss(A)}{\partial A}&=\frac{\partial (Y^TY-2A^TX^TY+A^TX^TXA)}{\partial A}\\
& 2B^TA\rightarrow 2A \; and \; B^TAB\rightarrow 2AB \;if\;A\;is\;symmetric\\
&=0-2X^TY+2X^TXA\\
&=-2X^TY+2X^TXA
\end{aligned}$$

Since $\frac{\partial Loss(A)}{\partial A}=0$, we can get:  

$$\begin{aligned}
&\Rightarrow -2X^TY+2X^TXA=0\\
&\Rightarrow 2X^TXA=2X^TY\\
& Multiply (X^TX)^{-1}\\
&\Rightarrow A=(X^TX)^{-1}X^TY
\end{aligned}$$


## Examples

[A comparison of Least Square Method and Gradient Descent on Linear Regression](../attachments/LinearRegression.py)  
(This is from [Tommy Huang](https://medium.com/@chih.sheng.huang821/%E7%B7%9A%E6%80%A7%E5%9B%9E%E6%AD%B8-linear-regression-3a271a7453e).)
