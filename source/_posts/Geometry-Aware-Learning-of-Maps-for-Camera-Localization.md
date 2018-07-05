---
title: Geometry-Aware Learning of Maps for Camera Localization
date: 2018-07-05 19:58:13
tags:
	- Paper-Reading
	- CVPR'18
	- Deep Learning
	- Localization
---

## Introduction

​	使用卷积神经网络解决定位问题。网络的主要任务是memorize一个地图，给出一张图像需要给出这个图像对应的pose，文章中没有提到内参处理（大部分深度学习解视觉的工作都不关心内参），但是代码实现里面有去畸变的过程。

## Base Line

​	PoseNet 15在Inception net上fine tune来回归出一个三维pose，其中三个参数是position，四个参数是quaternion，并且对四元数的模长不加约束，暴力回归也取得了效果，但是误差很大。并且发现position与quaternion的误差比例参数与场景大小有关。

​	PoseNet 17使用reprojection error以及自适应学习误差比例，提高了点儿精度。

## Major Contribution

### Step 1（MapNet）

​	将原来绝对误差损失函数的基础上增加相对误差的损失函数：同时给两张图像让mapnet给出两个pose，这两个pose算一个相对pose，与groundtruth的相对pose做一个损失函数。

​	这样可以让两张相近图像的pose有相对约束，解空间相对更平滑。（其实这里明显存在严重的overfitting，咋7Scenes这个dataset上表现的尤为明显，Saliency Map各种乱跳）

​	这个image pairs是一个video clip中每张图像与第一张图像组成的

### Step 2（MapNet++）

​	在Step 1的基础上增加unlabeled data进行self supervised learning。使用VO算法算出来一个相对pose，然后使用值作为相对pose的约束。还可以用GPS、IMU等廉价信息来做self supervised learning。

### Step 3（MapNet+PGO）

​	这一步只在最后test的时候做，连续取十帧图像预测pose，每个pose给出一个常数Variance做PGO

### Details

​	代码里面test set和validation set在Step 2的时候混掉了。而且paper里面给出跑100个epoch要跑好久好久（GTX 1060）

​	RobotCar数据集图像裁剪到了256大小，DSO来作为VO在同样大的图像上跑。对brightness，saturation，hue and contrast进行图像数据增强。这种办法对跨季节、跨时间的测试数据很必要。

​	quaternion使用log q来学习（李代数，SLAM里面常用的）

## Problems

​	可视化出来的saliency map在7Scenes场景中还是可以看到前后跳跃，不一致。

​	RobotCar数据集中行人、车辆对定位的扰动很明显。