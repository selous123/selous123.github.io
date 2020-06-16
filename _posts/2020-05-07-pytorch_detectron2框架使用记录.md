---
title: detectron2框架使用记录
date:  2020-05-07 16:12:13 +0800
category: computer-vision 
tags: deep-learning object-detection
excerpt: 目标检测模型框架detectron2使用记录
comment: True
mathjax: True
---

### 1. 框架安装
#### 1.1. Linux环境下 detectron2 框架安装
2020/05/07 推荐手动编译安装

Offical Link for Installation: https://github.com/facebookresearch/detectron2/blob/master/INSTALL.md

```
git clone https://github.com/facebookresearch/detectron2.git
python setup.py build develop
```

查看本地是否设置CUDA_HOME变量
```
python -c 'import torch; from torch.utils.cpp_extension import CUDA_HOME; print(torch.cuda.is_available(), CUDA_HOME)'
```

输出格式为:
**True 地址(/usr/local/cuda).**

#### 1.2. Kernel not compiled with GPU support?

问题链接：　https://github.com/facebookresearch/detectron2/issues/55

本人遇到的问题：在设置变量时，使用
CUDA_HOME = $CUDA_HOME:/usr/local/cuda. 导致CUDA_HOME环境变量实际为":/usr/local/cuda", 并不是"/usr/local/cuda"。代码识别不了路径，就会识别不出GPU,然后使用CPU编译。

### 2. 框架使用
测试faster-rcnn代码是否可以运行成功:
```
python main.py --config-file configs/PascalVOC-Detection/faster_rcnn_R_50_FPN.yaml --num-gpus 1 SOLVER.IMS_PER_BATCH 4 SOLVER.BASE_LR 0.01
```
#### 2.1 加载数据 (Load Data)
##### a) 预定义数据集
1. 通过环境变量　DETECTRON2_DATASETS, 设置数据集路径。例如：
```
os.environ["DETECTRON2_DATASETS"] = "/store/dataset/VOC2007/VOC_trainval"
```

2. 对于标准数据集，数据集文件夹格式需满足如下格式：　[标准数据集文件夹格式](https://github.com/facebookresearch/detectron2/tree/master/datasets)

3. 官方读取/加载数据集的代码实现： [官方代码实现](https://github.com/facebookresearch/detectron2/tree/master/detectron2/data)

4. Detectron2 加载数据到dataloader的[Pipeline详解](https://www.cnblogs.com/marsggbo/p/11727556.html)

##### b) 自定义数据集



#### 2.2 定义模型 (Model Defining)
##### a) 预定义模型
##### b) 自定义模型


### 3. 常用目标识别模型

#### 3.1 YoLo v3

模型输出: [b, bbox, bbox_attr]:[b, 10647, 5 + classes_num]

bbox_attr: xyhw, object_confidence(框内是否有物体), cls_confidence(物体类别的概率)