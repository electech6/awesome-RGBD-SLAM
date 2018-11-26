# Visual_SLAM_Collection
Visual SLAM open sources，good papers，famous research institutions
[TOC]

# RGB-D SLAM

## 背景介绍

RGB-D SLAM主要用于稠密三维重建。

在消费级深度相机出现之前，想要采用普通相机实现实时稠密三维重建比较困难。微软2010年发布了Kinect之后，基于深度相机的稠密三维重建掀起了研究热潮。早期比较有代表性的工作是2011年微软的Newcombe（单目稠密重建算法DTAM 的作者）、Davison等大牛发表在SIGGRAPH上的KinectFusion算法，算是该领域的开山之作。KinectFusion算法首次实现了基于廉价消费类相机的实时刚体重建，在当时是非常有影响力的工作，它极大的推动了实时稠密三维重建的商业化进程。下图所示是几款消费级深度相机。

![img](https://mmbiz.qpic.cn/mmbiz_png/rqpicxXx8cNnYjbuicGEZ7HsrTEat60VPR2PkYmMCEJouUTwZ0PrD1mXRHYV1e1sndA74dSvZY6xHP8ibpjWUok2g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

KinectFusion之后，陆续出现了Kintinuous，ElasticFusion，InfiniTAM，BundleFusion等非常优秀的工作。其中2017年斯坦福大学提出的BundleFusion算法，可以说是目前基于RGB-D相机进行稠密三维重建效果最好的方法了。

## Bundle fusion

### 基本信息

[项目官网](http://graphics.stanford.edu/projects/bundlefusion/)

[论文](https://arxiv.org/pdf/1604.01093.pdf)

[代码](https://github.com/niessner/BundleFusion)

[算法详细解读](https://mp.weixin.qq.com/s?__biz=MzIxOTczOTM4NA==&mid=2247485487&idx=1&sn=068e04d652578deb55e93b1a32fa9a21&chksm=97d7edb8a0a064ae265e9beb0f337f36fd3470a14ce97701cb16710608e0f1899c3d03b196ff&scene=21#wechat_redirect)

![bundlefusion](C:\Users\cxl5766\Desktop\bundlefusion.gif)

### 算法流程

Bundle fusion算法具体流程图如下：

- 输入：RGB-D相机采集的对齐好的color+depth的数据流，作者使用的是structure sensor+iPad输出的30Hz，分辨率为640x480的序列图像。
- 输出：重建好的三维场景。

![img](https://mmbiz.qpic.cn/mmbiz_png/rqpicxXx8cNnYjbuicGEZ7HsrTEat60VPRg6UNMSmb7wEXiaFr9nWnnzcubwyKt50cV85cO948a1eDJPs3LDeTPog/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 算法优缺点

#### 优点

1. 重建重建效果在所有RGB-D SLAM中 top 3.
2. 使用持续的local to global分层优化，去除了时域跟踪的依赖。
3. 不需要任何显示的loop closure检测。因为每一帧都和历史帧相关，所以其实包含了持续的隐式的loop closure。
4. 支持在GPU上实时鲁棒跟踪，可以在跟踪失败时移动到重建成功的地方进行relocalization，匹配上后继续跟踪。

#### 缺点

1. 由于成像传感器存在噪音，稀疏关键点匹配可能产生小的局部误匹配。这些误匹配可能会在全局优化中传播，导致误差累积。
2. 上述效果图片都是在作者提供的公开数据集上的效果，该数据集采集的场景纹理比较丰富，光照也比较适中。而实际重建时效果和所使用深度相机的性能、待重建场景的纹理丰富程度关系很大。对于办公室这种简洁风格的场景效果会下降很多，还有很多可改进的地方。
3. 目前算法需要两块GPU才能实时运行。

# InfiniTAM

## 基本信息

[官网](http://www.robots.ox.ac.uk/~victor/infinitam/)

[代码](https://github.com/victorprad/InfiniTAM)

![InfiniTAM](C:\Users\cxl5766\Desktop\InfiniTAM.gif)

# ORBSLAM V2

## 基本信息

[代码](https://github.com/raulmur/ORB_SLAM2)

[论文（RGB-D）](https://128.84.21.199/pdf/1610.06475.pdf)

![ORBSLAM2](C:\Users\cxl5766\Desktop\ORBSLAM2.gif)

# RTAB-Map

## 基本信息

[官网](http://introlab.github.io/rtabmap/)

[代码](https://github.com/introlab/rtabmap)

![RTAB-MAP](C:\Users\cxl5766\Desktop\RTAB-MAP.gif)

# Kintinuous

[代码](https://github.com/mp3guy/Kintinuous)

[论文](http://thomaswhelan.ie/Whelan14ijrr.pdf)

![Kintinuous](C:\Users\cxl5766\Desktop\Kintinuous.gif)

# ElasticFusion

[代码](https://github.com/mp3guy/ElasticFusion)

[论文](http://www.thomaswhelan.ie/Whelan16ijrr.pdf)

![ElasticFusion](C:\Users\cxl5766\Desktop\ElasticFusion.gif)

# rgbdslam_v2

[代码](https://github.com/felixendres/rgbdslam_v2)

[论文](http://www2.informatik.uni-freiburg.de/~endres/files/publications/felix-endres-phd-thesis.pdf)

![RGBDSLAMV2](C:\Users\cxl5766\Desktop\RGBDSLAMV2.png)

持续更新。。。



