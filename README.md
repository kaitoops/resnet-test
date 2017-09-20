DeepLab
=====
机器学习、深度学习、量化投资，好用的工具、实用的工具，尽在DeepLab ! 

ResNet
---------
ResNet的直连结构在一般的深度神经网络上应用效果测试

### 背景
ResNet 诞生于一个美丽而简单的观察：为什么非常深的网络在增加更多层时会表现得更差？

直觉上推测，更深度的网络不会比更浅度的同类型网络表现更差吧：
假设我们已经构建了一个 n 层网络，并且实现了一定准确度。那么一个 n+1 层网络至少也应该能够实现同样的准确度---只要简单复制前面 n 层，再在最后一层增加一层恒等映射就可以了。然而实际情况是，这些更深度的网络基本上都会表现得更差。

ResNet 的作者 何凯明博士 将这些问题归结成了一个单一的假设：恒等映射是难以学习的。而且他们提出了一种修正方法：不再学习从 x 到 H(x) 的基本映射关系，而是学习这两者之间的差异，也就是「残差（residual）」。然后，为了计算 H(x)，我们只需要将这个残差加到输入上即可。

假设残差为 F(x)=H(x)-x，那么现在我们的网络不会直接学习 H(x) 了，而是学习 F(x)+x。

这就带来了著名 ResNet（残差网络）模块：
![github](https://github.com/junliangliu/resnet-test/blob/master/resnet.png "")

### 问题
ResNet是在卷积神经网络上实现的，但其添加恒等映射、基于残差学习的思想应该是普适的：ResNet的直连结构能够解决任何深度神经网络的梯度弥散问题，即可通过添加ResnNet直连结构有效的训练一般的神经网络。

本文目的正式通过添加ResnNet直连结构，验证ResnNet在一般神经网络上的有效性。通过本文测试可直接观察到两个现象：

（1）深度网络的梯度弥散

（2）添加ResnNet直连结构的深度网络仍能够有效训练

### 实验环境
keras theano

数据集angle.csv：简单的分类问题：通过三条边的长度判断是否是直接三角形，是标签为1，否则为0。数据集共10000条数据，其中5000个为直角三角形。

### 网络结构对比
原始20层全连接网络                                           增加resnet的全连接网络
![github](https://github.com/junliangliu/resnet-test/blob/master/%E7%BB%93%E6%9E%84%E5%AF%B9%E6%AF%94%E5%9B%BE.png "对比")

### 代码结构
restnet-test.ipynb restnet对比测试脚本。

angle.csv 数据集。

### 实验结果
sigmoid激活函数+20层全连接神经网络：
Epoch 200/200
6000/6000 - loss: 0.6932 - acc: 0.5033 - val_loss: 0.6932 - val_acc: 0.4950

sigmoid激活函数+20层全连接神经网络 + resnet直连结构：
Epoch 200/200
6000/6000  - loss: 0.4864 - acc: 0.8128 - val_loss: 0.5081 - val_acc: 0.8005

relu激活函数+20层全连接神经网络：
Epoch 200/200
6000/6000 - loss: 0.6931 - acc: 0.5033 - val_loss: 0.6932 - val_acc: 0.4950

relu激活函数+20层全连接神经网络 + resnet直连结构：
Epoch 200/200
6000/6000  - loss: 0.1484 - acc: 0.9500 - val_loss: 0.1633 - val_acc: 0.9453

结论：

（1）20层全连接网络，无论是sigmoid还是relu激活函数，模型没有得到有效训练，梯度消失了

（2）添加resnet直连结构的20层全连接网络，仍然能够有效训练，准确度确实有较大提高

### 参考：
https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650729930&idx=1&sn=5f81864de64075de1a460e622baea239&chksm=871b29b4b06ca0a23e2bef560abf09dad1b7d48d7b657cb2cd47f59573638be74278013ea913#rd
