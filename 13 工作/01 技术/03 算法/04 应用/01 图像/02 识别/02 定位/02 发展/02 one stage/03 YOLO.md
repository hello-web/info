
# YOLO

## 介绍

目标：

- 提高速度，继承 OverFeat

介绍：

- YOLO 算法是基于图像的全局信息进行预测的，整体结构简单
- 通过将输入图像重整到 448×448 像素固定尺寸大小，并划分图像为 7×7 网格区域
- 通过卷积神经网络提取特征训练，直接预测每个网格内的边框坐标和每个类别置信度，训练时采用 P-Relu 激活函数。

效果：

- 速度能达到每秒 45 帧

缺点：

- 存在定位不准以及召回率不如基于区域提名方法的问题
- 对距离很近的物体和很小的物体检测效果不好
- 泛化能力相对较弱。


## 特点

**补充**

<span style="color:red;">**!**</span>

- 原理
- 用法，注意点，
- 与 fast rcnn 、faster-rcnn 的对比。为什么使用
- loss function 是什么
- 定位精度不够的时候怎么办？怎么调参，怎么提高效果

