---
layout: post
title: Typical Ideas in Neural Network
date: 2020-10-18
author: Brad
tags:  Machine_Learning DNN
comments: true
toc: true
pinned: false
---

本篇回顧那些典型深度學習模型主要的讓其強大的idea。

## 2012 AlexNet/Dropout
AlexNet開啟了近年深度學習大浪潮，結合GPU及dropout，且激活函式使用ReLu等等現今許多模組都已成為基本配備。

<!-- more -->

![](https://i.imgur.com/dF1bwaX.png)

### Papers
> * [ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks)
> * [Improving neural networks by preventing co-adaptation of feature detectors](https://arxiv.org/abs/1207.0580)
> * [One weird trick for parallelizing convolutional neural networks](https://arxiv.org/abs/1404.5997)

### Implementation
> * [Pytorch](https://pytorch.org/hub/pytorch_vision_alexnet/)
> * [TensorFlow](https://github.com/tensorflow/models/blob/master/research/slim/nets/alexnet.py)


## 2013 Reinforcement Learning - Atari
用神經網路來打遊戲，為基於像素來打遊戲的神經網路，而且能在不設定遊戲規則的情況下打七種不同的遊戲。特別的是，它用的是強化學習，旨在最大化獎勵而非預測標籤。
![](https://i.imgur.com/UBDHxFI.png)

### Papers
> * [Playing Atari with Deep Reinforcement Learning](https://arxiv.org/abs/1312.5602)

### Implementation
> * [PyTorch](https://pytorch.org/tutorials/intermediate/reinforcement_q_learning.html)
> * [TensorFlow](https://www.tensorflow.org/agents/tutorials/1_dqn_tutorial)

## 2014 Seq2Seq+Atten
在注意力的機制出現後，NLP領域才真正令人矚目。源於RNN類的架構(含LSTM, GRU)都會遇到序列過長，導致容易忘記較早的輸入，即是梯度爆炸或消失問題。注意力機制便是解決問題之道，透過神經網路「注意」特定詞並輸出來實現。
![](https://i.imgur.com/mtl8nnd.png)

### Papers
> * [Sequence to Sequence Learning with Neural Networks](https://arxiv.org/abs/1409.3215)
> * [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/abs/1409.0473)

### Implementation
> * [PyTorch](https://pytorch.org/tutorials/intermediate/seq2seq_translation_tutorial.html)
> * [TensorFlow](https://www.tensorflow.org/addons/tutorials/networks_seq2seq_nmt)

## 2014 Adam Optimizer
雖然說SGD為典型的優化演算法，但其因為有多參學可調，增加對挑選好參數的時間，故提出Adam，其能讓調參數步驟更少、更高的機率有更好的模型。
![](https://i.imgur.com/dDR1O89.png)

### Papers
> * [Adam: A Method for Stochastic Optimization](https://arxiv.org/abs/1412.6980)

### Implementation
> * [Python](https://d2l.ai/chapter_optimization/adam.html)
> * [PyTorch](https://pytorch.org/docs/master/_modules/torch/optim/adam.html)
> * [TensorFlow](https://github.com/tensorflow/tensorflow/blob/v2.2.0/tensorflow/python/keras/optimizer_v2/adam.py#L32-L281)

## 2015 Generative Adversarial Networks
生成模型的目標是生成假資料，內容為訓練兩個網路，分別是生成器和判別器，隨著訓練進行，生成器越能產生能欺騙判別器的資料。
![](https://i.imgur.com/gdTDTpE.jpg)

### Papers
> * [Generative Adversarial Networks](https://arxiv.org/abs/1406.2661)
> * [Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks](https://arxiv.org/abs/1511.06434)


### Implementation
> * [PyTorch](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html)
>* [TensorFlow](https://www.tensorflow.org/tutorials/generative/dcgan)

## 2015 Residual Networks
這是一種非RNN系列解決深度神經網路學習問題的網路，致使有使用Residual的深度的網路層學習目標為原輸入之「差」，讓網路得以更深卻不會效果下降。
![](https://i.imgur.com/UpAxMTq.png)

### Papers
> * [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)

### Implementation
> * [PyTorch](https://github.com/pytorch/vision/blob/master/torchvision/models/resnet.py)
> * [TensorFlow](https://github.com/tensorflow/tensorflow/blob/v2.2.0/tensorflow/python/keras/applications/resnet.py)


## 2017 Transformers
更一步改進了Attension類的模型，加入了Self-attension層且同時處理所有序列。Transformer不止表現好，還打敗了RNN模型。
> * [參考資料](http://jalammar.github.io/illustrated-transformer/)

![](https://i.imgur.com/uQFEFJz.png)

### Papers
> * [Attention is All You Need](https://arxiv.org/abs/1706.03762)

### Implementation
> * [PyTorch](https://pytorch.org/tutorials/beginner/transformer_tutorial.html)
> * [TensorFlow](https://www.tensorflow.org/tutorials/text/transformer)
> * [HuggingFace Transformers](https://github.com/huggingface/transformers)

## BERT and Fine-tuned models
一個會辨識貓的模型，能夠更容易分狐狸的圖片。同樣，一個學習了預測下一句子是什麼的模型，能夠更好地學習新任務。
較特別的是，BERT是預測句子中的mask(正確詞或隨機替換成其它詞)以及兩句是否為上下句。而且在數據並不需要人工標記(純文本)
![](https://i.imgur.com/2Y5Yosi.png)

### Papers
> * [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/abs/1810.04805)

### Implementation
> * [微調BERT的HuggingFace實現](https://huggingface.co/transformers/training.html)

## 2020 未來
* 更好的資料(數據)及更大的模型能夠打敗==聰明的技術==
    * 如OpenAI的GPT-3
* 利用沒有標籤的數據 transfer learning
    * Contrastive  self-supervised learning
        * SimCLR