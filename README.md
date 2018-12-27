# RGB-D_SLAM_Collection
RGB-D SLAM open sources，good papers，famous research institutions

[TOC]

## 背景介绍

RGB-D SLAM主要用于稠密三维重建。

在消费级深度相机出现之前，想要采用普通相机实现实时稠密三维重建比较困难。微软2010年发布了Kinect之后，基于深度相机的稠密三维重建掀起了研究热潮。早期比较有代表性的工作是2011年微软的Newcombe（单目稠密重建算法DTAM 的作者）、Davison等大牛发表在SIGGRAPH上的KinectFusion算法，算是该领域的开山之作。KinectFusion算法首次实现了基于廉价消费类相机的实时刚体重建，在当时是非常有影响力的工作，它极大的推动了实时稠密三维重建的商业化进程。下图所示是几款消费级深度相机。

![img](https://mmbiz.qpic.cn/mmbiz_png/rqpicxXx8cNnYjbuicGEZ7HsrTEat60VPR2PkYmMCEJouUTwZ0PrD1mXRHYV1e1sndA74dSvZY6xHP8ibpjWUok2g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

KinectFusion之后，陆续出现了Kintinuous，ElasticFusion，InfiniTAM，BundleFusion等非常优秀的工作。其中2017年斯坦福大学提出的BundleFusion算法，可以说是目前基于RGB-D相机进行稠密三维重建效果最好的方法了。

# Bundle fusion

### 基本信息

[项目官网](http://graphics.stanford.edu/projects/bundlefusion/)

[论文](https://arxiv.org/pdf/1604.01093.pdf)

[代码](https://github.com/niessner/BundleFusion)

[算法详细解读](https://mp.weixin.qq.com/s?__biz=MzIxOTczOTM4NA==&mid=2247485487&idx=1&sn=068e04d652578deb55e93b1a32fa9a21&chksm=97d7edb8a0a064ae265e9beb0f337f36fd3470a14ce97701cb16710608e0f1899c3d03b196ff&scene=21#wechat_redirect)

![bundlefusion.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/bundlefusion.gif?raw=true)



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

![InfiniTAM.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/InfiniTAM.gif?raw=true)

## 算法优缺点

### 优点

- 提供Linux，iOS，Android平台版本
- CPU可以实时重建
- 重建效果还不错

### 缺点

- loop closure 不是很稳定，loop closure后经常无法消除累计漂移
- 没有提供带color的重建结果，需要自己写

# ORBSLAM V2

## 基本信息

[代码](https://github.com/raulmur/ORB_SLAM2)

[论文（RGB-D）](https://128.84.21.199/pdf/1610.06475.pdf)

![ORBSLAM2.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/ORBSLAM2.gif?raw=true)

## 简单介绍

### 背景

ORB-SLAM 是西班牙 Zaragoza 大学研究者开发的视觉 SLAM 系统。 它是一个完整的 SLAM 系统，包括视觉里程计、跟踪、回环检测，是一种完全基于稀疏特征点的单目 SLAM 系统，同时还有单目、双目、RGBD 相机的接口。其核心是使用 ORB (Orinted FAST and BRIEF) 作为整个视觉 SLAM 中的核心特征。

ORB-SLAM 基本延续了 PTAM 的算法框架,但对框架中的大部分组件都做了改进, 归纳起来主要有 4 点:

1. ORB-SLAM 选用了 ORB 特征, 基于 ORB 描述量的特征匹配和重定位, 都比 PTAM 具有更好的视角不变性。此外, 新增三维点的特征匹配效率更高, 因此能更及时地扩展场景。扩展场景及时与否决定了后续帧是否能稳定跟踪。
2. ORBSLAM 加入了循环回路的检测和闭合机制, 以消除误差累积。系统采用与重定位相同的方法来检测回路(匹配回路两侧关键帧上的公共点), 通过方位图 (Pose Graph) 优化来闭合回路。
3. PTAM 需要用户指定 2 帧来初始化系统, 2 帧间既要有足够的公共点, 又要有足够的平移量. 平移运动为这些公共点提供视差 (Parallax) , 只有足够的视差才能三角化出精确的三维位置。ORB-SLAM 通过检测视差来自动选择初始化的 2 帧。
4. PTAM 扩展场景时也要求新加入的关键帧提供足够的视差, 导致场景往往难以扩展. ORB-SLAM 采用一种更鲁棒的关键帧和三维点的选择机制——先用宽松的判断条件尽可能及时地加入新的关键帧和三维点, 以保证后续帧的鲁棒跟踪; 再用严格的判断条件删除冗余的关键帧和不稳定的三维点，以保证 BA 的效率和精度。

ORB-SLAM2 在 ORB-SLAM 的基础上，还支持标定后的双目相机和 RGB-D 相机。双目对于精度和鲁棒性都会有一定的提升。ORB-SLAM2 是基于单目，双目和 RGB-D 相机的一套完整的 SLAM 方案。它能够实现地图重用，回环检测和重新定位的功能。无论是在室内的小型手持设备，还是到工厂环境的无人机和城市里驾驶的汽车，ORB-SLAM2 都能够在标准的 CPU 上进行实时工作。ORB-SLAM2 在后端上采用的是基于单目和双目的光束法平差优化（BA）的方式，这个方法允许米制比例尺的轨迹精确度评估。此外，ORB-SLAM2 包含一个轻量级的定位模式，该模式能够在允许零点漂移的条件下，利用视觉里程计来追踪未建图的区域并且匹配特征点。

### 系统框架

![image](https://img-blog.csdn.net/20161114115114018)

 它是由三大块、三个流程同时运行的。第一块是跟踪，第二块是建图，第三块是闭环检测。

1. 跟踪（Tracking）

这一部分主要工作是从图像中提取 ORB 特征，根据上一帧进行姿态估计，或者进行通过全局重定位初始化位姿，然后跟踪已经重建的局部地图，优化位姿，再根据一些规则确定新关键帧。

1. 建图（LocalMapping）

这一部分主要完成局部地图构建。包括对关键帧的插入，验证最近生成的地图点并进行筛选，然后生成新的地图点，使用局部捆集调整（Local BA），最后再对插入的关键帧进行筛选，去除多余的关键帧。

1. 闭环检测（LoopClosing）

这一部分主要分为两个过程，分别是闭环探测和闭环校正。闭环检测先使用 WOB 进行探测，然后通过 Sim3 算法计算相似变换。闭环校正，主要是闭环融合和 Essential Graph 的图优化。

## 算法优缺点

### 优点

- 一个代码构造优秀的视觉 SLAM 系统，非常适合移植到实际项目。
- 采用 g2o 作为后端优化工具，能有效地减少对特征点位置和自身位姿的估计误差。
- 采用 DBOW 减少了寻找特征的计算量，同时回环匹配和重定位效果较好。重定位：比如当机器人遇到一些意外情况之后，它的数据流突然被打断了，在 ORB-SLAM 算法下，可以在短时间内重新把机器人在地图中定位。
- 使用了类似「适者生存」的方案来进行关键帧的删选，提高系统追踪的鲁棒性和系统的可持续运行。
- 提供最著名的公共数据集（ KITTI 和 TUM 数据集）的详尽实验结果，以显示其性能。
- 可以使用开源代码，并且还支持使用 ROS。

### 缺点

- 构建出的地图是稀疏点云图。只保留了图像中特征点的一部分作为关键点，固定在空间中进行定位，很难描绘地图中的障碍物的存在。
- 初始化时最好保持低速运动，对准特征和几何纹理丰富的物体。
- 旋转时比较容易丢帧，特别是对于纯旋转，对噪声敏感，不具备尺度不变性。
- 如果使用纯视觉 slam 用于机器人导航，可能会精度不高，或者产生累积误差，漂移，尽管可以使用 DBoW 词袋可以用来回环检测。最好使用 VSLAM+IMU 进行融合，可以提高精度上去，适用于实际应用中机器人的导航。

# RTAB-Map

## 基本信息

[官网](http://introlab.github.io/rtabmap/)

[代码](https://github.com/introlab/rtabmap)

![RTAB-MAP.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/RTAB-MAP.gif?raw=true)

## 简介

RTAB-Map（ R**eal-**T**ime **A**ppearance-**B**ased **Mapping）用于基于外观的实时建图， 是一个通过内存管理方法实现回环检测的开源库。它限制地图的大小以使得回环检测始终在固定的时间限制内处理，从而满足长期和大规模环境在线建图要求。 

从2013年开始并于2013年作为开源库发布，RTAB-Map已经扩展到完整的基于图的SLAM方法，目前RTAB-Map已经发展成为一个跨平台的独立C ++库和一个ROS包，允许用户使用不同的机器人和传感器实现和比较各种3D和2D对于各种应用的解决方案。

[本文](https://mp.weixin.qq.com/s?__biz=MzI5MTM1MTQwMw==&mid=2247502325&idx=1&sn=724e23271a82a74797efbbe60e9b4a55&chksm=ec1377f1db64fee769ce9c8087c2149979bc64260a511ba2f6bb4f5fb8ea54cf4e2f350e36c9&scene=0#rd)介绍了RTAB-Map的扩展版本及其在大量流行的真实数据集进行定量和定性的比较，（例如KITTI, EuRoC, TUM RGB-D, MIT Stata Center）。从自主导航应用的实用角度概述视觉和激光雷达SLAM配置的优势和局限性。

## 算法流程

### 1、RTAB-Map 整体描述

![img](https://mmbiz.qpic.cn/mmbiz_png/O60Uib8kfuuibDPu9MVgkf9licGJQSnU2Z6OuS4WmbZybFwzVVkJ0ae1DmpvbMB0CruF8EUOlr9eWwGW8uleXTIhg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

*RTAB-Map ROS节点的框图。所需输入是：TF，用于定义传感器相对于机器人底座的位置; 来自任何来源的里程计（可以是3DoF或6DoF）; 其中一种相机输入（一个或多个RGB-D图像，或双目立体图像），且带有相应的校准消息。可选输入：2D激光的雷达扫描，或3D激光的点云。然后，来自这些输入的所有消息被同步并传递给graph-SLAM算法。输出的是：Map Data，包含最新添加的节点（带有压缩传感器数据）和Graph; Map Graph，没有任何数据的纯图;TF，矫正过的里程计; 可选的OctoMap（3D占用栅格地图）; 可选的稠密点云地图; 可选的2D占用栅格地图。*

### 2、视觉里程计

![img](https://mmbiz.qpic.cn/mmbiz_png/O60Uib8kfuuibDPu9MVgkf9licGJQSnU2Z63d1Jx55OlaicFPPXvUibtPtuicSEJ0m7qhIibKWRKQ0OKlmVXrbEAjgThg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

*RGBD里程计和双目立体视里程计的ROS节点框图。TF定义相机相对于机器人基座的位置，并作为输出来发布机器人基座的里程计变换。对于RGB-D相机或立体相机，管道是相同的，除了多计算一步相应的立体深度信息，以便稍后确定检测到的特征的深度。可以有两种里程计的方法：绿色的帧到帧（F2F）方法，红色的帧到地图（F2M）方法。*



### 3、激光里程计

![img](https://mmbiz.qpic.cn/mmbiz_png/O60Uib8kfuuibDPu9MVgkf9licGJQSnU2Z6wQ5v5kt8KNqFKqy2plicMLGtwUnC2dv7fFsibUntwmeFDePseibUAC3jA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

*ICP里程计ROS节点框图。TF定义激光雷达相对于机器人基座的位置，并作为输出以发布机器人基座的里程计变换。可以有两种里程计的方法：绿色的扫描到扫描（S2S）方法，红色的扫描到地图（S2M）方法。 这些方法还可以选择使用恒速模型（粉红色）或其他里程计（蓝色）进行运动预测。 对于后者，输入测距的校正发布在TF上。*



### 4、局部地图

![img](https://mmbiz.qpic.cn/mmbiz_png/O60Uib8kfuuibDPu9MVgkf9licGJQSnU2Z6A6R5mCibs9NLHFiaiaU6RuVDibXW3HH7JAwB0MoOwGdLQjkpcUfdrmWFwA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

*STM的局部占用栅格地图创建。 依赖的参数（由椭圆显示），可选用激光扫描和点云输入（由棱形显示），局部占用栅格地图可以是2D或3D。*

### 5、全局地图。

![img](https://mmbiz.qpic.cn/mmbiz_png/O60Uib8kfuuibDPu9MVgkf9licGJQSnU2Z6hBFZVnP9zm0lIoibRWOxDJflplD9mvYEJS6rFgL771aXt3iaic2oxILBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

*全局地图集成。依赖于局部地图创建地图的类型（见上图）。当只有3D局部占用栅格地图可用于生成3D占用栅格地图（OctoMap）及其2D地图。*

## 算法优缺点

### 优点

- 定位精度很准
- 支持视觉、激光传感器
- 支持跨平台、ROS
- 在线处理

### 缺点

- 鲁棒性不是很好，如果建图时间和重定位时间间隔得比较久，或者光线变化都很明显的话，重定位会失败
- 点云网格化用possion重建，不是主流的TSDF，速度会慢
- 重建效果不如 bundle fusion，但是也还不错

# SLAMRecon

[官网](http://irc.cs.sdu.edu.cn/SLAMRecon/)

[代码](https://github.com/HaoLi-China/SLAMRecon)

![SLAMRecon.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/SLAMRecon.gif?raw=true)

# Kintinuous

[代码](https://github.com/mp3guy/Kintinuous)

[论文](http://thomaswhelan.ie/Whelan14ijrr.pdf)

![Kintinuous.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/Kintinuous.gif?raw=true)

# ElasticFusion

[代码](https://github.com/mp3guy/ElasticFusion)

[论文](http://www.thomaswhelan.ie/Whelan16ijrr.pdf)

![ElasticFusion.gif](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/ElasticFusion.gif?raw=true)

# rgbdslam_v2

[代码](https://github.com/felixendres/rgbdslam_v2)

[论文](http://www2.informatik.uni-freiburg.de/~endres/files/publications/felix-endres-phd-thesis.pdf)

![RGBDSLAMV2.png](https://github.com/electech6/RGB-D-SLAM-Collection/blob/master/RGBDSLAMV2.png?raw=true)

持续更新。。。



