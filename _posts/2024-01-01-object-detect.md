# 目标检测算法

### R-CNN系列

- **算法原理**：
  - **R-CNN**：先使用选择性搜索生成大量候选区域，再将每个候选区域缩放到固定大小，输入到预训练的卷积神经网络中提取特征，最后利用支持向量机进行分类，同时使用回归器精确定位。
  - **Fast R-CNN**：在一个网络中进行区域特征提取和分类，通过引入感兴趣区域池化层（RoIPooling），对候选区域提取特征，提高了速度。
  - **Faster R-CNN**：引入区域提议网络（RPN），与Fast R-CNN共享卷积层，让候选区域生成成为网络的一部分，进一步提高了检测速度和性能。
- **高星GitHub库**：
  - https://github.com/rbgirshick/py-faster-rcnn
  - https://github.com/jwyang/faster-rcnn.pytorch

### SSD

- **算法原理**：使用一个单一的网络完成目标定位与分类，在不同尺度的特征图上进行密集预测，对每个位置的多个先验框进行目标类别和边界框的预测，通过损失函数来优化模型，包括定位损失和分类损失。
- **高星GitHub库**：
  - https://gitcode.com/gh_mirrors/ss/ssd-models
  - https://github.com/amdegroot/ssd.pytorch

### YOLO系列

- **算法原理**：将输入图像划分为一个个格子，每个格子负责预测该区域内的目标，每个格子会预测多个边界框和这些边界框的置信度，同时每个边界框还会预测属于各个类别的条件概率，最后使用非极大值抑制去除多余的边界框。
- **高星GitHub库**：
  - https://github.com/ultralytics/yolov5
  - https://github.com/ultralytics/yolov8

### Mask R-CNN

- **算法原理**：在Faster R-CNN基础上扩展，增加了一个分支用于预测目标的分割遮罩。先通过RPN生成候选区域，再用RoIAlign对候选区域提取特征，接着分别进行分类、边界框回归和分割遮罩的预测。
- **高星GitHub库**：
  - https://gitcode.com/gh_mirrors/ma/mask-rcnn-keras
  - https://github.com/facebookresearch/maskrcnn-benchmark

### RetinaNet

- **算法原理**：使用骨干网络提取图像特征，通过特征金字塔网络（FPN）融合不同尺度的特征图，在每个尺度的特征图上使用分类子网络和回归子网络进行目标类别和边界框的预测，核心是焦点损失函数，用来解决类别不平衡问题。
- **高星GitHub库**：
  - https://github.com/fizyr/keras-retinanet
  - https://github.com/facebookresearch/Detectron/tree/master/projects/RetinaNet
