线性回归
========

模型
--------

输入与输出之间的线性关系

数据集
------

含有特征和标签的数据集合

训练数据/测试数据

损失函数
-------
非负误差，通常为平方函数
优化函数
------

* 解析解：用公式直接表达

* 数值解：优化算法有限次迭代模型参数尽可能降低损失函数的值

* 学习率：每次优化中，学习的步长大小

向量相加
-------

* 向量按元素逐一做标量加法
* 做矢量加法

[cdoe](code/向量计算.py)


线性回归模型从零开始的实现
--------------------------

能够更好的理解模型和神经网络底层的原理

[cdoe](code/线性回归模型从零实现.py)

线性回归模型使用pytorch的简洁实现
-------

能够更加快速地完成模型的设计与实现

[code](code/线性回归模型使用pytorch的简洁实现.py)


softmax和分类模型
=======
softmax基本概念
-----
* 分类

使用离散数值表示类别

* 神经网络图
softmax回归同线性回归一样，也是一个单层神经网络
softmax回归的输出层也是一个全连接层。

* 输出问题

1.输出层的范围不统一

2.离散值不缺丢范围的输出值之间的误差

使用softmax将输出值变换成正且和为一的分布概率

* 计算效率

1.将单样本进行矢量计算来表达

2.小批量数据做矢量运算

交叉熵损失函数
-----

* 平方损失估计

我们只需要其中一个比其他预测值和大就行了，不管其他两个预测值为多少，类别预测均正确。
而平方损失则过于严格，俩种=者分类正确的情况下损失会升高。

* 交叉熵

交叉熵只关心对正确类别的预测概率，因为只要其值足够大，就可以确保分类结果正确。当然，遇到一个样本有多个标签时，例如图像里含有不止一个物体时，我们并不能做这一步简化。但即便对于这种情况，交叉熵同样只关心对图像中出现的物体类别的预测概率。

获取Fashion-MNIST训练集和读取数据
---
[code](code/获取Fashion-MNIST训练集和读取数据.py)

softmax从零开始的实现
------
[code](code/softmax从零开始的实现.py)
    
softmax的简洁实现
-----
[code](code/softmax的简洁实现.py)

多层感知机
====

一个隐藏层可含有多个隐藏单元，隐藏层和输出层均是全连接层，也就是将隐藏层的输出直接作为输出层的输入
虽然神经网络引入了隐藏层，却依然等价于一个单层神经网络

激活函数
----

对隐藏变量使用按元素运算的非线性函数进行变换，然后再作为下一个全连接层的输入。这个非线性函数被称为激活函数

* ReLU函数

保留正数元素，并将负数元素清零

* Sigmoid函数

可以将元素的值变换到0和1之间

* tanh函数

函数可以将元素的值变换到-1和1之间

激活函数选择
----
ReLu函数是一个通用的激活函数，目前在大多数情况下使用。但是，ReLU函数只能在隐藏层中使用。

用于分类器时，sigmoid函数及其组合通常效果更好。由于梯度消失问题，有时要避免使用sigmoid和tanh函数。

在神经网络层数较多的时候，最好使用ReLu函数，ReLu函数比较简单计算量少，而sigmoid和tanh函数计算量大很多。

在选择激活函数的时候可以先选用ReLu函数如果效果不理想可以尝试其他激活函数。

多层感知机
-----
多层感知机就是含有至少一个隐藏层的由全连接层组成的神经网络，且每个隐藏层的输出通过激活函数进行变换。多层感知机的层数和各隐藏层中隐藏单元个数都是超参数

多层感知机从零开始的实现
------
[code](code/多层感知机从零开始的实现.py)


多层感知机pytorch实现
----
[code](code/多层感知机pytorch实现.py)

文本预处理
====
文本是一类序列数据，一篇文章可以看作是字符或单词的序列，本节将介绍文本数据的常见预处理步骤，预处理通常包括四个步骤：

* 读入文本
* 分词
* 建立字典，将每个词映射到一个唯一的索引（index）
* 将文本从词的序列转换为索引的序列，方便输入模型

将词转为索引
---
使用字典，我们可以将原文本中的句子从单词序列转换为索引序列

缺点：
* 标点符号通常可以提供语义信息，但是我们的方法直接将其丢弃了
* 类似“shouldn't", "doesn't"这样的词会被错误地处理
* 类似"Mr.", "Dr."这样的词会被错误地处理

spaCy:
---
```
import spacy
nlp = spacy.load('en_core_web_sm')
doc = nlp(text)
print([token.text for token in doc])
```

NLTK:
---
```
from nltk.tokenize import word_tokenize
from nltk import data
data.path.append('/home/kesci/input/nltk_data3784/nltk_data')
print(word_tokenize(text))
```

语言模型
====
一段自然语言文本可以看作是一个离散时间序列，给定一个长度为 T 的词的序列 w1,w2,…,wT ，语言模型的目标就是评估该序列是否合理，即计算该序列的概率
n元语法
---
序列长度增加，计算和存储多个词共同出现的概率的复杂度会呈指数级增加。 n 元语法通过马尔可夫假设简化模型，马尔科夫假设是指一个词的出现只与前面 n 个词相关，即 n 阶马尔可夫链（Markov chain of order  n ），如果 n=1 ，那么有 P(w3∣w1,w2)=P(w3∣w2) 

* 读取数据集
* 建立字符索引

时序数据的采样
---
* 随机采样

每次从数据里随机采样一个小批量。其中批量大小batch_size是每个小批量的样本数，num_steps是每个样本所包含的时间步数。 在随机采样中，每个样本是原始序列上任意截取的一段序列，相邻的两个随机小批量在原始序列上的位置不一定相毗邻

* 相邻采样

在相邻采样中，相邻的两个随机小批量在原始序列上的位置相毗邻

循环神经网络
====

能够捕捉截至当前时间步的序列的历史信息，就像是神经网络当前时间步的状态或记忆一样。

从零开始实现循环神经网络
----
[code](code/从零开始实现循环神经网络.py)

one-hot向量
---

假设词典大小是 N ，每次字符对应一个从 0 到 N−1 的唯一的索引，则该字符的向量是一个长度为 N 的向量，若字符的索引是 i ，则该向量的第 i 个位置为 1 ，其他位置为 0 。下面分别展示了索引为0和2的one-hot向量，向量长度等于词典大小

困惑度
---

* 最佳情况下，模型总是把标签类别的概率预测为1，此时困惑度为1；

* 最坏情况下，模型总是把标签类别的概率预测为0，此时困惑度为正无穷；

* 基线情况下，模型总是预测所有类别的概率都相同，此时困惑度为类别个数。

循环神经网络的简介实现
---

[code](code/循环神经网络的简介实现.py)

nn.RNN的以下几个构造函数参数：

* input_size - The number of expected features in the input x
* hidden_size – The number of features in the hidden state h
* nonlinearity – The non-linearity to use. Can be either 'tanh' or 'relu'. Default: 'tanh'
* batch_first – If True, then the input and output tensors are provided as (batch_size, num_steps, input_size). Default: False

forward函数的参数为：

* input of shape (num_steps, batch_size, input_size): tensor containing the features of the input sequence.
* h_0 of shape (num_layers * num_directions, batch_size, hidden_size): tensor containing the initial hidden state for each element in the batch. Defaults to zero if not provided. If the RNN is bidirectional, num_directions should be 2, else it should be 1.

forward函数的返回值是：

* output of shape (num_steps, batch_size, num_directions * hidden_size): tensor containing the output features (h_t) from the last layer of the RNN, for each t.
* h_n of shape (num_layers * num_directions, batch_size, hidden_size): tensor containing the hidden state for t = num_steps.


