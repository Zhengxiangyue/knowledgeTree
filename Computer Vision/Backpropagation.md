## Introduction

Backpropagation(反向传播) 是训练深度模型时常用的深度算法。在现代神经网络中，它可以使梯度下降训练的速度比一些传统的方法快一千万倍。本来要计算二十万年的模型，现在只用几周。

除了在深度学习中使用到，backpropagation 在其他领域的计算中也是一个强大的工具，包括天气预报中分析数值稳定性，只是名字不同而已，实际上这个算法在不同领域已经被“重新发明”了至少十多次，一个比较 general 的名字叫做 reverse-mode differentiation反向模式划分

简单来讲，它是一种快速求导的方法，是一种非常巧妙的方法。不知是在机器学习的领域，在其他方面也是不错的工具。

## Computational Graphs

画“计算图”是理解数学表达式的一个不错的方法，举个例子，考虑表达式
$$
e = (a + b)*(b+1)
$$
里面有三个操作符：两个加号和一个乘号。为了帮助我们理解，引入两个中间变量，c 和 d
$$
c = a + b\\
 d = b + 1\\
 e = c *d
$$
要想建立“计算图”，我们把每一个操作符同它的两个变量画成一个节点，如果一个节点的结果是另一个节点的输入，就画一个箭头过去![tree-def](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/tree-def.png)

这种图像在计算机科学领域非常常见，特别是在讨论一个有功能的程序的时候，它和依赖图、调用图息息相关。它也是著名的深度学习框架 theano 的核心抽象结构

