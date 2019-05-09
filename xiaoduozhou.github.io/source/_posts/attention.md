---
title:  seq2seq 及 attention机制介绍
date: 2019-05-09 13:55:58
tags: [attention,神经网络,pytorch，seq2seq,端到端]
categories:
- 深度学习
mathjax: true
---

转自：[知乎](https://zhuanlan.zhihu.com/p/40920384)

### 关于seq2seq
实现链接 ： [seq2seq](https://github.com/xiaoduozhou/nlp_examples/tree/master/NLG/seq2seq)

&emsp;&emsp;seq2seq 是一个Encoder–Decoder 结构的网络，它的输入是一个序列，输出也是一个序列， Encoder 中将一个可变长度的信号序列变为固定长度的向量表达，Decoder 将这个固定长度的向量变成可变长度的目标的信号序列。

**大体框架** 如下：

{% asset_img 1.PNG %}

**细节框架**如下：

{% asset_img 2.PNG %}

### 核心公式：
输入： $x = (x_{1},...,x_{T_{x}})$
输出： $y = (y_{1},...,y_{T_{y}})$

(1) $h_{t} = RNN_{enc}(x_{t},h_{t-1})$，Encoder方面接受的是每一个单词word embedding，和上一个时间点的hidden state。输出的是这个时间点的hidden state。

{% asset_img 3.PNG %}


(2) $s_{t} = RNN_{dec}(y_{t}^{'},s_{t-1})$，Decoder方面接受的是目标句子里单词的word embedding，和上一个时间点的hidden state。

{% asset_img 4.PNG %}


(3) $$c_{i}=\sum_{j=1}^{T_{x}}a_{ij}h_{j}$$，context vector是一个对于encoder输出的hidden states的一个加权平均。

(4)  $$a_{ij} = \frac{exp(e_{ij})}{\sum_{k=1}^{T_{x}}exp(e_{ik})  }$$ ，每一个encoder的hidden states对应的权重。

(5) $e_{ij} = score(s_{i},h_{j})$，通过decoder的hidden states加上encoder的hidden states来计算一个分数，用于计算权重(4)

{% asset_img 5.PNG %}

{% asset_img 6.PNG %}


(6) $s_{t}^{'}=tanh(W_{c}[c_{t};s_{t}])$ ，将context vector 和 decoder的hidden states 串起来。

(7) $p(y_{t}|y_{<t},x) = softmax(W_{s}s_{t}^{'})$，计算最后的输出概率。

{% asset_img 7.PNG %}


### Attention介绍
&emsp;&emsp;在luong中提到了三种score的计算方法。这里图解前两种：
{% asset_img 71.PNG %}


#### Attention score function: dot

{% asset_img 8.PNG %}


&emsp;&emsp;输入是encoder的所有hidden states H: 大小为(hid dim, sequence length)。decoder在一个时间点上的hidden state， s： 大小为（hid dim, 1）。
1.  旋转H为（sequence length, hid dim) 与s做点乘得到一个 大小为(sequence length, 1)的分数。
2.  对分数做softmax得到一个合为1的权重。
3.  将H与第二步得到的权重做点乘得到一个大小为(hid dim, 1)的context vector。


#### Attention score function: general

{% asset_img 9.PNG %}


&emsp;&emsp;输入是encoder的所有hidden states H: 大小为(hid_dim1, sequence length)。decoder在一个时间点上的hidden state， s： 大小为（hid_dim2, 1）。此处两个hidden state的纬度并不一样。

1. 旋转H为（sequence length, hid dim1) 与 Wa [大小为 (hid dim1, hid dim 2)] 做点乘， 再和s做点乘得到一个 大小为(sequence length, 1)的分数
2. 对分数做softmax得到一个合为1的权重。
3. 将H与第二步得到的权重做点乘得到一个大小为(hid dim, 1)的context vector。

