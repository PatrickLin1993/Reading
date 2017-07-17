# Where to Buy It: Matching Street Clothing Photos in Online Shops

## 摘要

* 该文所做的工作就是将街拍图片中的衣物同网络在线商店的衣物图片进行检索匹配。
* 数据集为从25家不同的网络零售商店提取了404683张衣物图片，20357张街拍图片，提供了总共39479对街拍图片和在线商店图片的匹配。
* 作者提出了三种不同的方法实现该匹配过程，包括两个深度学习基本线方法，和一个从街拍图片到在线商店图片的相似性学习方法。
* 实验证明了作者的相似性学习方法表现优于另外两个使用现存基于深度学习表达的基本线方法。

作者提供了数据集的下载：[Dataset]([http://www.tamaraberg.com/street2shop/)

## 方法

### 1. 全图检索 (Whole Image Retrieval)

使用 AlexNet 对街拍图片中提前标注好的窗体内的衣物进行特征提取， 然后计算在线商店衣物图全图的CNN特征。最后根据余弦距离判断相似度。

[关于 AlexNet](http://www.jianshu.com/p/58168fec534d?from=androidqq)

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/AlexNet.png)

### 2. 似物性采样检索 (Object Proposal Retrieval)

相比**方法1**，对在线商店衣物图不再进行全图处理，而是进行似物性采样，该文采用选择性搜索(selective serach) ，从图中提取出100个物体。最后同样根据余弦距离判断相似度。

### 3. 相似性学习 (Simularity Learning)

作者设计了一个三层全连接的网络模型：

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/sl.png)

其中 soft-max 激活函数：

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/soft-max.png)

训练过程采用交叉熵损失函数 (cross-entropy loss)，相似性比较基于SIFT特征（对于相似性比较，实验发现SIFT特征优于L2以及余弦距离）。

[关于交叉熵函数](https://hit-scir.gitbooks.io/neural-networks-and-deep-learning-zh_cn/content/chap3/c3s1.html#)

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/crossentropy.png)

* `n`为训练数据个数，前一个`y`代表输出，后一个`^y`代表预期输出。
* 输出越接近输出预期输出，则结果越趋近于0，反之为更大的正值。

最后作者把衣物分成了5个大类，并根据每个类对网络模型进行了微调：

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/slnet.png)

### 实验结果

作者给出了效果图，前三个为成功匹配，后三个为失败匹配。

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/res1.png)

以及 top-k 实验比较图：

![](https://github.com/PatrickLin1993/Reading/blob/master/Where%20to%20Buy%20It/pics/res2.png)

