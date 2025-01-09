# 人工智能案例学习

- pytorch样例
  ```
  git clone  git@github.com:pytorch/examples.git pytorch_examples
  ```
- tensorrt样例
  ```
  git clone  git@github.com:NVIDIA/retinanet-examples.git
  ```
- nvidia样例
  ```
  git clone  git@github.com:NVIDIA/DeepLearningExamples.git NVIDIA_DeepLearningExamples
  ```

# .cuda()与.to(device)的区别

to(device)可以让指定cpu和gpu

cuda只能指定gpu

# 深度学习优化方法

[ML 2021 Spring](https://speech.ee.ntu.edu.tw/~hylee/ml/2021-spring.php)
李宏毅 机器学习官网 ppt

### 梯度相关优化

- **梯度归一化**：将算出来的梯度除以minibatch size，有助于稳定训练过程，使不同规模的batch在梯度更新上更具一致性。
- **梯度裁剪（clip c）**：
  - **原理**：限制最大梯度，计算梯度向量的范数（如value = sqrt(w1^2 + w2^2 + ……)），当该范数超过设定阈值（如5，10，15等）时，计算衰减系数，将梯度向量的范数调整至等于阈值，避免梯度爆炸问题，保证训练的稳定性。

### 防止过拟合方法

- **Dropout**：
  - **应用场景及效果**：对小数据防止过拟合效果较好，一般设置为0.5。在小数据上，结合随机梯度下降（SGD）使用时，在大部分实验中能明显提升效果。
  - **放置位置**：对于循环神经网络（RNN），建议放置在“输入→RNN”与“RNN→输出”的位置，相关参考论文为[http://arxiv.org/abs/1409.2329](http://arxiv.org/abs/1409.2329)。

### 激活函数选择

除了在特定需要将输出限制成0 - 1的gate之类的地方，尽量避免使用sigmoid激活函数，可选用tanh或者ReLU等激活函数。原因在于sigmoid函数只有在-4到4的区间里才有较大的梯度，在此区间外梯度接近0，容易导致梯度消失问题；并且输入为0均值时，sigmoid的输出不是0均值的，不利于训练。

### 超参数调整

- **RNN相关**：RNN的维度（dim）和嵌入大小（embedding size）一般从128上下开始调整；batch size一般从128左右开始尝试调整，合适的batch size很重要，并非越大越好。
- **数据处理**：尽量对数据做shuffle操作，打乱数据顺序，有助于模型更好地学习数据特征，提升泛化能力。

### 特定网络结构参数设置

对于长短期记忆网络（LSTM），其遗忘门（forget gate）的bias，用1.0或者更大的值作为初始值，能够取得更好的结构效果，参考论文为[http://jmlr.org/proceedings/papers/v37/jozefowicz15.pdf](http://jmlr.org/proceedings/papers/v37/jozefowicz15.pdf)。

### 归一化方法

Batch Normalization（批量归一化）据说可以提升效果，其具体的原理和优势可参考论文[http://arxiv.org/abs/1505.00387](http://arxiv.org/abs/1505.00387)。

# torchvision.transforms

## torchvision.transforms.ToTensor()

[pytorch](https://so.csdn.net/so/search?q=pytorch&spm=1001.2101.3001.7020)在加载数据集时都需要对数据记性transforms转换，其中最常用的就是torchvision.transforms.ToTensor()函数，但初学者可以认为这个函数只是把输入数据类型转换为pytorch的Tensor（int64）类型，其实不然，该函数内部的具体转换步骤为：

1、将图片转化成内存中的存储格式；

2、将字节以流的形式输入，转化成Tensor类型；

3、对Tensor进行reshape；

4、对Tensor进行permute（2,0,1），因为pytorch的维度和图片维度不一样；

5、将Tensor中的每个元素除以255；6、输出该Tensor数据。

注：以后用了这个函数之后别再想着给数据在除以255归一化一下了。。。

## torchvision.transforms.ToPILImage()

pytorch在将[Tensor](https://so.csdn.net/so/search?q=Tensor&spm=1001.2101.3001.7020)类型数据转换为图片数据时，需要用到这个torchvision.transforms.ToPILImage()函数，该函数的作用就是把Tensor数据变为完整原始的图片数据（保存后可以直接双击打开的那种），函数内部的具体转换步骤为：

> 1、将Tensor的每个元素乘以255；
> 2、将数据由Tensor转化成Uint8；
> 3、将Tensor转化成numpy的ndarray类型；
> 4、对ndarray对象做permute (1, 2, 0)的转置，因为pytorch的维度和图片维度不一样；
> 5、将ndarray对象转化成PILImage数据格式；
> 6、输出该PILImage数据（save后可以直接打开）。
>

# 分布

## 熵

其中 $H(P)$为熵 (entropy), $H(P, Q) $为 $P$ 和 $Q$的交叉熵 $(cross entropy)$。

在信息论中，熵 $H ( P )$ 表示对来自 $P $ 的随机变量进行编码所需的最小字节数，而交叉熵 $H(P,Q) $则表示使用基于$Q $的编码对来自$P$的变量进行编码所需的字节数。

因此，$KL $散度可认为是使用基于$ Q$的编码对来自 $P$的变量进行编码所需的 “额外” 字节数; 显然，额外字节数必然非负，当且仅当 $P=Q $时额外字节数为零。

## KL散度

又称相对熵或者信息散度，用于度量两个概率分布之间的差异

给定两个概率分布$P$和$Q$，$P$和$Q$之间的$KL$散度为

$$
KL(P||Q)=\int_{-\infty}^{\infty}p(x)log\frac{p(x)}{q(x)}dx
$$

$$
KL(P||Q)=\int_{-\infty}^{\infty}p(x)log{p(x)}dx-\int_{-\infty}^{\infty}p(x)log{q(x)}dx=-H(P)+H(P,Q)
$$

## 贝叶斯概率

## 高斯分布和混合高斯随机变量

连续型标量随机变量x的概率密度函数

$$
p(x)=\frac{1}{(2\pi\sigma^2)^\frac{1}{2}}exp[-\frac{1}{2}(\frac{x-\mu}{\sigma})^2]\approx\mathcal{N}(x;\mu,\sigma^2)
$$

服从高斯分布。

使用精度参数（r=方差的倒数=$\frac{1}{\sigma^2}$,$\sigma=\frac{1}{\sqrt{r}}$）代替方差后，

$$
p(x)=\sqrt{\frac{r}{2\pi}}exp[-\frac{r}{2}({x-\mu})^2]\approx\mathcal{N}(x;\mu,\sigma^2)
$$
