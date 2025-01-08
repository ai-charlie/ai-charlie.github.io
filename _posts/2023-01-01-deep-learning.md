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
