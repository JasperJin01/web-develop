---
title: Transformer的学习（1）
date: 2024-3-31
layout: post 
---



# 李宏毅 Self-attention

李宏毅老师《机器学习》课程笔记-4.1 Self-attention[^1]（写了很多东西，挺干货的。包括最重要的attention计算过程）。







# Attention核心思想

Attention核心思想[^2]：Attention的思想如同它的名字一样，就是“注意力”，**在预测结果时把注意力放在不同的特征上**。

举个例子。比如在预测“我妈今天做的这顿饭真好吃”的情感时，如果只预测正向还是负向，那真正影响结果的只有“真好吃”这三个字，前面说的“我妈今天做的这顿饭”基本没什么用，如果是直接对token embedding进行平均去求句子表示会引入不少噪声。所以引入attention机制，让我们可以根据任务目标赋予输入token不同的权重，理想情况下前半句的权重都在0.0及，后三个字则是“0.3, 0.3, 0.3”，在计算句子表示时就变成了：

最终表示 = 0.01x我+0.01x妈+0.01x今+0.01x天+0.01x做+0.01x的+0.01x这+0.01x顿+0.02x饭+0.3x真+0.3x好+0.3x吃

怎么用知道了，那核心就在于这个权重怎么计算。通常我们会将输入分为query(Q), key(K), value(V)三种：

1. 先用Q和K计算权重$\alpha$，会用softmax对权重归一化： $\alpha = softmax(f(QK))$
2. 再用权重对结果加权： $out = \Sigma \alpha_i * V_i$

![img](https://pic3.zhimg.com/80/v2-bd434c5710af5fd2d51c19c2b615ae62_1440w.webp)

上文中QK的具体运算f有多种方法，常见的有加性attention和乘性attention：

- $f(Q.K) = tanh(W_1Q + W_2K)$
- $f(Q,K) = QK^T$

再深入理解下去，这种机制其实做的是寻址（addressing），也就是模仿中央处理器与存储交互的方式将存储的内容读出来，可以看一下[李宏毅老师的课程](https://speech.ee.ntu.edu.tw/~tlkagk/courses_MLSD15_2.html)。







# Transformer模型原理









---

[^1]: https://zhuanlan.zhihu.com/p/521380855 
[^2]: https://zhuanlan.zhihu.com/p/43493999 这篇文章后面还介绍了attention在NLP中最早的应用：Seq2seq+attention。 

