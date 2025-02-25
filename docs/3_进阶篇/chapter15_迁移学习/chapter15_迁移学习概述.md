# 第 15 章 迁移学习

作者: 张伟 (Charmve)

日期: 2021/05/22

## 目录

  - [第 15 章 迁移学习](https://charmve.github.io/computer-vision-in-action/#/chapter3/chapter3)
    - [15.1 概述](#151-迁移学习概述)
      - [15.1.1 背景](#1511-背景)
      - [15.1.2 定义及分类](#1512-定义及分类)
      - [15.1.3 关键点](#1513-关键点)
    - [15.2 基于实例的迁移](#152-基于实例的迁移)
    - [15.3 基于特征的迁移](#153-基于特征的迁移)
    - [15.4 基于共享参数的迁移](#154-基于共享参数的迁移)
    - [15.5 深度学习和迁移学习结合](#155-深度学习和迁移学习结合)
    - [15.7 实战项目 2 - 蚂蚁和蜜蜂的分类问题](chapter15_迁移学习的应用.md)
      - [15.7.1 迁移学习在计算机视觉领域的应用](chapter15_迁移学习的应用.md#1571-迁移学习在计算机视觉领域的应用)
      - [15.7.2 实战项目: 蚂蚁和蜜蜂的分类问题](chapter15_迁移学习的应用.md#1572-实战项目-蚂蚁和蜜蜂的分类问题)
    - [小结](#小结)
    - [参考文献](#参考文献)

## 15.1 迁移学习概述
### 15.1.1 背景
随着越来越多的机器学习应用场景的出现，而现有表现比较好的监督学习需要大量的标注数据，标注数据是一项枯燥无味且花费巨大的任务，所以迁移学习受到越来越多的关注。

传统机器学习(主要指监督学习)有两大特点：

- 基于同分布假设
- 需要大量标注数据

然而实际使用过程中不同数据集可能存在一些问题，比如

- 数据分布差异

- 标注数据过期
  训练数据过期，也就是好不容易标定的数据要被丢弃，有些应用中数据是分布随着时间推移会有变化

如何充分利用之前标注好的数据（废物利用），同时又保证在新的任务上的模型精度？

基于这样的问题，所以就有了对于``迁移学习``的研究。如图15.1 展示了迁移学习的发展历程。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219725-8a12da80-bb19-11eb-9381-eaf6b999e384.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="66%">
</div>
图15.1 迁移学习的发展历程

### 15.1.2 定义及分类
> Transfer Learning Definition:
Ability of a system to recognize and apply knowledge and skills learned in previous domains/tasks to novel domains/tasks.
> <br>迁移学习的定义: 系统识别并应用先前领域/任务学到的知识和技能到新颖领域/任务的能力。

#### 目标
将某个领域或任务上学习到的知识或模式应用到不同但相关的领域或问题中。

#### 主要思想
从相关领域中迁移标注数据或者知识结构、完成或改进目标领域或任务的学习效果。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219729-8da66180-bb19-11eb-8b51-31c2a2ff0364.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="66%">
</div>

图15.2 迁移学习的"举一反三"


人在实际生活中有很多迁移学习，比如学会骑自行车，就比较容易学摩托车，学会了C语言，在学一些其它编程语言会简单很多。那么机器是否能够像人类一样举一反三呢？如图15.2所示。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219733-91d27f00-bb19-11eb-930e-9e56804273c3.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.3 跨领域情感分类的概率分配问题

上图（如图15.3所示）是一个商品评论情感分析的例子，图中包含两个不同的产品领域：books 图书领域和 furniture 家具领域；在图书领域，通常用“broad”、“quality fiction”等词汇来表达正面情感，而在家具领域中却由“sharp”、“light weight”等词汇来表达正面情感。可见此任务中，不同领域的不同情感词多数不发生重叠、存在领域独享词、且词汇在不同领域出现的频率显著不同，因此会导致领域间的概率分布失配问题。

**迁移学习的形式定义及一种分类方式**

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219737-94cd6f80-bb19-11eb-93ea-c984824bc446.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.4 迁移学习的形式定义及一种分类方式

如图15.4所示，迁移学习根据特征空间X、边际概率分布P(X)、标签空间的不同，分为**异构迁移学习**和**同构迁移学习**。异构迁移学习分为异构特征空间和异构类别空间，而同构迁移学习又分为数据集偏移、领域适配、多任务学习，领域适配是本书的主要关注点。

迁移学习里有两个非常重要的概念：

- **域（Domain）**： 可以理解为某个时刻的某个特定领域，比如书本评论和电视剧评论可以看作是两个不同的domain。
- **任务（Task）**： 就是要做的事情，比如情感分析和实体识别就是两个不同的task。

### 15.1.3 关键点

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219739-972fc980-bb19-11eb-934f-89b22b998beb.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.5 迁移学习的关键点

如图15.5所示，迁移学习有三个关键点：用什么迁移、如何进行迁移和何时适合迁移，具体解释如下：

1. 研究可以用哪些知识在不同的领域或者任务中进行迁移学习，即**不同领域之间有哪些共有知识可以迁移**。

2. 研究在找到了迁移对象之后，针对具体问题所采用哪种迁移学习的特定算法，即**如何设计出合适的算法来提取和迁移共有知识**。

3. **研究什么情况下适合迁移**，迁移技巧是否适合具体应用，其中涉及到负迁移的问题。

4. 当领域间的概率分布差异很大时，上述假设通常难以成立，这会导致严重的负迁移问题。
    - 负迁移是旧知识对新知识学习的阻碍作用，比如学习了三轮车之后对骑自行车的影响，和学习汉语拼音对学英文字母的影响
    - 研究如何利用正迁移，避免负迁移

## 15.2 基于实例的迁移

图15.6所示，基于实例的迁移学习研究的是，如何从源领域中挑选出，对目标领域的训练有用的实例，比如对源领域的有标记数据实例进行有效的权重分配，让源域实例分布接近目标域的实例分布，从而在目标领域中建立一个分类精度较高的、可靠地学习模型。

因为，迁移学习中源领域与目标领域的数据分布是不一致，所以源领域中所有有标记的数据实例不一定都对目标领域有用。戴文渊等人提出的 TrAdaBoost 算法就是典型的基于实例的迁移。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219743-9e56d780-bb19-11eb-8569-f54d6f491e9a.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.6 基于实例的迁移


## 15.3 基于特征的迁移
### 15.3.1 特征选择

如图15.7所示，基于特征选择的迁移学习算法，关注的是如何找出源领域与目标领域之间共同的特征表示，然后利用这些特征进行知识迁移。

### 15.3.2 特征映射

基于特征映射的迁移学习算法，关注的是如何将源领域和目标领域的数据从原始特征空间映射到新的特征空间中去。

这样，在该空间中，源领域数据与的目标领域的数据分布相同，从而可以在新的空间中，更好地利用源领域已有的有标记数据样本进行分类训练，最终对目标领域的数据进行分类测试。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219747-a4e54f00-bb19-11eb-8737-01f3ccef08b4.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.7 特征映射

## 15.4 基于共享参数的迁移
基于共享参数的迁移研究的是如何找到源数据和目标数据的空间模型之间的共同参数或者先验分布，从而可以通过进一步处理，达到知识迁移的目的，假设前提是，学习任务中的的每个相关模型会共享一些相同的参数或者先验分布。

## 15.5 深度学习和迁移学习结合
<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219751-a878d600-bb19-11eb-9c04-cd32dcc2cec1.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.8 预训练模型

如图15.8所示，深度学习需要大量的高质量标注数据，Pre-training + fine-tuning 是现在深度学习中一个非常流行的trick，尤其是以图像领域为代表，很多时候会选择预训练的ImageNet对模型进行初始化。

下面将主要通过一些paper对深度学习中的迁移学习应用进行探讨。

### 15.5.1 Pre-training+Fine-tuning
2014年 Bengio 等人在NIPS上发表论文 《How transferable are features in deep neural networks》 <sup>[1]</sup>，研究深度学习中各个layer特征的可迁移性（或者说通用性）。

文章中进行了如下图所示的实验，有四种模型：

- Domain A上的基本模型BaseA；
- Domain B上的基本模型BaseB；
- Domain B上前n层使用BaseB的参数初始化（后续有frozen和fine-tuning两种方式）；
- Domain B上前n层使用BaseA的参数初始化（后续有frozen和fine-tuning两种方式）；

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219756-ab73c680-bb19-11eb-9e02-6759e8a4ca42.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.9 Pre-training + Fine-tuning

如图15.9所示，将深度学习应用在图像处理领域中时，会观察到第一层（first-layer）中提取的features基本上是类似于Gabor滤波器(Gabor filters)和色彩斑点(color blobs)之类的。

通常情况下第一层与具体的图像数据集关系不是特别大，而网络的最后一层则是与选定的数据集及其任务目标紧密相关的；文章中将第一层feature称之为一般(general)特征，最后一层称之为**特定(specific)特征**。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219757-ae6eb700-bb19-11eb-821c-5aace83b6521.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

如图15.10所示，根据四种模型的性能对比，可以得出影响迁移学习能力的两个主要问题：
1. 网络断开带来的最优化困难问题；
2. 由于高层网络层中特定化的特征表示使得在迁移过程中所带来的性能损失。

两个因素哪一个占支配位置，取决于迁移的特征是在网络的bottom、meddle 还是 top 。

![image](https://user-images.githubusercontent.com/29084184/119458437-54672f00-bd6f-11eb-9229-0b27ea2f8c77.png)

图15.10 四种模型的性能对比

下面给出在项目中的几点经验总结：

- 特征迁移使得模型的泛化性能有所提升，即使目标数据集非常大的时候也是如此。
- 随着参数被固定的层数n的增长，两个相似度小的任务之间的transferability gap的增长速度比两个相似度大的两个任务之间的transferability gap增长更快 两个数据集越不相似特征迁移的效果就越差。
- 即使从不是特别相似的任务中进行迁移也比使用随机filters（或者说随机的参数）要好。
- 使用迁移参数初始化网络能够提升泛化性能，即使目标task经过了大量的调整依然如此。

### 15.5.2 DANN ([Domain-Adversarial Neural Network](https://arxiv.org/abs/1505.07818))

这篇 paper<sup>3</sup> 将近两年流行的对抗网络思想引入到迁移学习中，从而提出了DANN。

<div align=center>
<img src="https://user-images.githubusercontent.com/29084184/119219761-b2023e00-bb19-11eb-9778-28a68e74bccf.png" alt="迈微AI研习社是一个专注AI领域的开源组织，作者系CSDN博客专家，主要分享机器学习算法、计算机视觉等相关内容，每周研读顶会论文，持续关注前沿技术动态。底部有菜单分类，关注我们，一起学习成长。" width="86%">
</div>

图15.11 Domain-Adversarial Neural Network 框架

如图15.11中所展示的即为DANN的结构图，框架由feature extractor、label predictor和domain classifier三个部分组成，并且在feature extractor和domain classifier 之间有一个gradient reversal layer；其中domain classifier只在训练过程中发挥作用

- DANN将领域适配和特征学习整合到一个训练过程中，将领域适配嵌入在特征表示的学习过程中；所以模型最后的分类决策是基于既有区分力又对领域变换具有不变性的特征。
- 优化特征映射参数的目的是为了最小化label classifier的损失函数，最大化domain classifier的损失函数，前者是为了提取出具有区分能力的特征，后者是为了提取出具有领域不变性的特征，最终优化得到的特征兼具两种性质。

## 小结

## 参考文献
[1] How transferable are features in deep neural networks（NIPS2014 Bengio et al.）

[2] Learning and Transferring Mid-Level Image Representations using Convolutional Neural Networks（CVPR2014 Oquab.et al.）

[3] Domain Adaptation for Large-Scale Sentiment Classification: A Deep Learning Approach（ICML2011 Glorot. Bengio.et al.） https://arxiv.org/abs/1505.07818

[4] Marginalized denoising autoencoders for domain adaptation (ICML2012 Chen et al.)

[5] Domain-Adversarial Training of Neural Networks（JMLR2016 Ganin.et al.）
