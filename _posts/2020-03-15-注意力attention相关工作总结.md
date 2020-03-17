---
title: 图像注意力机制工作总结 
date:  2020-03-15 21:34:13 +0800
category: architecture
tags: deep-learning architecture
excerpt: 总结注意力机制attention中比较有趣的工作
comment: True
mathjax: True
---
<font color="red"><b>注意力机制</b></font>：想法起源于对人类视觉系统机制的研究。 人类视觉系统在观察图像时，一般会格外注意自己想要关注的区域。而传统方式训练出的神经网络，对图片的全部区域其实是等价处理的。虽然神经网络学习到了图片的特征来进行分类，但是这些特征在神经网络“眼里”没有差异，神经网络并不会过多关注某个“区域”。但是人类注意力是会集中在这张图片的一个区域内，而其他的信息受关注度会相应降低。

<font color="red"><b>注意力机制的作用</b>：</font>一方面注意力机制可以指导神经网络关注特定区域的特征信息，另一方面注意力机制还能帮助研究人员理解神经网络时通过关注哪些区域而做到正确分类的，进而更好的理解神经网络的机制 [2]。 

<font color="red"><b>注意力机制的做法</b>：</font>深度学习与视觉注意力机制结合的研究工作，大多数是集中于使用掩码(mask)来形成注意力机制。掩码的原理是通过定义注意力分支，然后通过分支学习到不同特征位置的权重---权重大的即表明该位置的特征重要性大--然后将图片数据中关键的特征标识出来，通过学习训练，让深度神经网络学到每一张新图片中需要关注的区域，也就形成了注意力。

<font color="red"><b>注意力机制的分类</b>：</font> 图像注意力机制可以分为软注意力（soft attention）和强注意力（hard attention）。软注意力是确定性的注意力，一般通过网络可以直接生成，而且软注意力是可微的，所以就可以和神经网络一起训练。而强注意力是关注于每个点可能延伸出的区域，该注意力方式一般采用随机的预测过程，本身强注意力机制是不可微的，所以一般通过强化学习来训练。软注意力的容易实现（加一个注意力mask生成的分支）和可微性（与神经网络一起训练）使得其在研究领域更受青睐。在实际的工程项目中，软注意力也更容易被开发人员接受。

<font color="red"><b>软注意力机制的分类</b>：</font> 根据注意力关注的域不同，软注意力机制也可以大致分为空间域，通道域和混合域。下面分别通过起源文章，简单介绍这三个方向。

<font color="red"><b>问题定义：</b></font> 假设网络学到的某一层特征为: $X \in \mathcal{R}^{[c_{\text{in}}, \text{H}, \text{W}]}$, 我们希望注意力分支可以学习到重要性掩码$\text{mask}$， 然后将特征的重要性作用到特征上，也就是：

$$
\hat{X} = \text{mask} * X
$$

注意力机制的重点就是如何构建网络学习到合适的 $\text{mask}$。

### 1. 空间注意力 (Spatial Attention)
空间注意力机制一般是通过注意力分支生成空间上的特征重要性矩阵：$\text{mask} \in \mathcal{R}^{[\text{H}, \text{W}]}$ 对于不同Channel相同位置的特征点分配相同的重要性。

a). 设计思路

<font color="red"> Spatial Transformer Networks（STN）</font>模型$^{[3]}$是15年NIPS上的文章，这篇文章应该是空间注意力机制第一次被应用到图像领域，但是当时文章作者并没有提出注意力机制的概念，原始论文声明的文章要点是：将原始图片中的空间信息变换到另一个空间中并保留了关键信息。

<center><img src="https://selous123.github.io/assets/img/blog-attention/stn.png" width="70%" height="auto"/>

<span>图. Spatial Transformer Networks 框架图</span></center>

从上图可以看出，STN 通过一个两层的神经网络 (i.e., Localization net) 生成一个 $\text{mask}$ 然后将 $\text{mask}$ 作用到原始特征 $U$ 上。不同的是，其实首先通过矩阵乘法（也就是文章题目的 Transformer）作用到空间区域点阵 (The regular spatial grid) $G$ 上生成特征权重矩阵。而后面常用的空间注意力机制相当于他的简化版本--直接通过神经网络生成特征权重矩阵 $\text{mask}$。

b). 代码实现（简化版）

```
class SALayer(nn.Module):
    def __init__(self, channel, reduction=16):
        super(SALayer, self).__init__()
        kernel_size = 7
        self.compress = ChannelPool()
        self.spatial = BasicConv(1, 1, kernel_size, stride=1, padding=(kernel_size-1) // 2, relu=False)
        #self.spatial = nn.Conv2d(8, 8,kernel_size=7,stride=1,padding=(kernel_size-1) // 2,bias=False)
        self.channel = channel
    def forward(self, x):
        x_compress = self.compress(x)
        x_out = self.spatial(x_compress)
        scale = torch.sigmoid(x_out) # broadcasting
        return x * scale
```

空间注意力机制设计思路是让网络关注到不同位置的重要性，其在不同通道之间会赋予相同的权重。理论上空间注意力机制可以放在网络的任何位置。但是由于其设计思路的限制，把空间注意力集成在网络内部就显得与设计初衷相悖，因为经过不同卷积核输出的不同特征图，所包含的信息量和重要性是不一样的。所以对于空间注意力，其最适合的位置还是输入层。

### 2. 通道注意力 (Channel Attention)
通道注意力机制是通过关注特征之间的不同 Channel 之间的重要性。通过生成一个 $\text{mask} \in \mathcal{R}^{[C_\text{in}]}$，然后作用到所有特征的通道上。

a). 设计思路
论文$^{[4]}$中提出了一个非常重要的SENet的模型结构，靠着这个模型他们获得了ImageNet的冠军，这个模型是非常有创造力的设计。

<center><img src="https://selous123.github.io/assets/img/blog-attention/senet.png" width="70%" height="auto"/>

<span>图. Channel Attention 框架图</span></center>

我们知道，卷积神经网络通过不同的卷积核与特征卷积会生成不同性质的特征图（如边缘，纹理等）。对于不同任务来说，这些特征图谱的重要性自然是不同的。所以通道注意力机制的动机是很明确的，而且实验结果在不同的任务上都会有很大的提升。

b). 代码实现

```
class CALayer(nn.Module):
    def __init__(self, channel, reduction=16):
        super(CALayer, self).__init__()
        # global average pooling: feature --> point
        self.avg_pool = nn.AdaptiveAvgPool2d(1)
        # feature channel downscale and upscale --> channel weight
        self.conv_du = nn.Sequential(
                nn.Conv2d(channel, channel // reduction, 1, padding=0, bias=True),
                nn.ReLU(inplace=True),
                nn.Conv2d(channel // reduction, channel, 1, padding=0, bias=True),
                nn.Sigmoid()
        )

    def forward(self, x):
        y = self.avg_pool(x)
        y = self.conv_du(y)
        return x * y
```


### 3. 混合注意力 (Hybrid Attention)

混合注意力机制是融合了上述两种工作的思路，在空间和通道域都会加权。文中生成的特征重要性矩阵：$\text{mask} \in \mathcal{R}^{[C_\text{in}, \text{H}, \text{W}]}$, 也就是会对所有的特征点都会赋予一个不同的权重。

但是文中提出直接这样加权是有问题的：
1. dot production with mask range from zero to one repeatedly will degrade the value of features in deep layers. 
2. soft mask can potentially break good property of trunk branch, for example, the identical mapping of Residual Unit.

所以他们提出了一个残差的结构。

$$H = (1 + M) * f(x)$$

其中 $M$ 是注意力模块的输出，$x$ 是输入，$f$ 是激活函数， $H$ 是最终残差结构输出的特征重要性掩码矩阵 $\text{mask}$。


<center><img src="https://selous123.github.io/assets/img/blog-attention/hattn.png" width="70%" height="auto"/>
<span>图. 基于残差的attention结构</span></center>

对于该工作的有效性，我是持怀疑态度的。因为直接对每个特征点加权是不好的，我个人认为是因为参数过多会导致网络出现严重的过拟合，所以对于文中给出的解释并没有产生共鸣。文中的效果提升到底是因为加深了网络还是设计的attention结构带来的提升也说不清楚。

### 4. 总结

基于图像注意机制工作的重点就是如何利用现有的特征去学习到重要性矩阵掩码 $\text{mask}$。后面一般follow这些工作的改进业主要是重点解决这个问题。如何在生成 $\text{mask}$ 时考虑到更多的空间信息。


<center> <font size="5"><b>Reference</b></font> </center>

[1]. 微信公众号 https://mp.weixin.qq.com/s/KKlmYOduXWqR74W03Kl-9A

[2]. Zhang, Quanshi, Ying Nian Wu, and Song-ChunZhu. "Interpretable Convolutional Neural Networks." In CVPR 2018.

[3]. Jaderberg, Max, Karen Simonyan, and AndrewZisserman. "Spatial transformer networks." In NIPS 2015.

[4]. Hu, Jie, Li Shen, and Gang Sun."Squeeze-and-excitation networks." In CVPR 2018.

[8] Wang, Fei, et al. "Residual attentionnetwork for image classification." In CVPR 2017.