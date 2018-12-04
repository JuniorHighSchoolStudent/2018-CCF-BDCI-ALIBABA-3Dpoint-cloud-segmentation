# 2018-BDCI---top5
======================================
# 主办方：中国计算机学会 & DataFountain & 阿里巴巴
----------------------------------------
## 赛题 自动驾驶三维点云分割
-------------------------------------------
### 比赛链接：https://www.datafountain.cn/competitions/314/details/rule
### 百度云盘下载链接（权重下载链接）：链接：https://pan.baidu.com/s/1ZwvoH2lRghaknokNe-u8yA  提取码：2nz0 
### 数据集解释：由于本次的数据集超过100G，无法上传百度云，这里我们只取了几个数据作为sample，应该可以使用训练赛数据集作为替代：https://www.datafountain.cn/competitions/326/details
### 训练权重：将maskrcnn文件夹放入FaterRCNN/train_log下,将ImageNet-R101-AlignPadding放入FasterRCNN下，本训练代码参考：https://github.com/tensorpack/tensorpack/tree/master/examples/FasterRCNN

#### 1.数据预处理：
##### 官方提供了pts，intensity，category三类点云数据，我们这里参考了Complex-YOLO: Real-time 3D Object Detection on Point Clouds的思路将三类点云数据分别归一化到0~1的范围后重组为三通道图片数组，作为我们的训练图像。我们的图片和标签制作过程详见代码making_training_data/pointcloud2RGB.py

#### 2.数据增强：
##### 鸟瞰图像有一个很大的特点，就是多方向性。传统图像数据集里面，道路目标姿态往往都是类似的，同时也不会有较大的倾斜。鸟瞰图数据集的这个问题就严重，道路目标的朝向东南西北都有可能的，因此训练集里的朝向应当要丰富，避免学习到的模型不具有泛性。 针对这一问题我们采取了下面几种数据增强方法：随机30°倍数旋转，随机水平翻转，随机平移。（均为线上数据增强)

#### 3.数据格式转换：
##### 将得到的数据制分别制作成VOC和COCO格式，便于训练。

#### 4.目标检测算法选择：
##### 对多类目标检测算法进行尝试后，最后敲定使用Faster-RCNN作为目标检测算法，得到0.2以上的检测精度。（所需环境和使用方法均已经写在两个txt文件中）

#### 5.调参过程：
##### 主要对目标nms阈值和目标置信度进行了调整，我们的结果可视化代码要使用matlab，在result_check/下

#### 6.最终复赛成绩：0.248

#### 后续可以考虑改进的点：
###### 1.我们本次比赛对数据的清洗和分析做的不够，实际上该数据集类间数量分布很不均匀，需要针对这个情况，对每个类别进行置信度调整，同时部分数据的标注也存在一定的问题，要进行部分数据的筛选。
###### 2.验证集的分割没做好，理想应该挑选5%的数据作为验证集，我们的验证集太小，缺乏代表性
###### 3.图像分割在这个赛题会比目标检测算法有着更好的精度，同时速度上也会有较大的优势。
###### 4.若使用图像分割的话，推断时间减少，就可以尝试在inference阶段使用TTA的思路，减少假阳性。


