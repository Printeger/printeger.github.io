---
title: LO/LIO Performence Experiment In Different Road Scenes
author: printeger
date: 2022-11-30 12:00:00 +0800
categories: [Project, SLAM]
tags: [LiDAR, SLAM, Experiment]
math: true
mermaid: true
---

- [1. 待测试：](#1-待测试)
  - [1.1 Lidar 里程计：](#11-lidar-里程计)
  - [1.2 Lidar-IMU 里程计](#12-lidar-imu-里程计)
- [2. 测试方法：](#2-测试方法)
- [3. 测试数据：](#3-测试数据)
- [4. 隧道场景（11.30）](#4-隧道场景1130)
- [5. 城市场景（12.01）](#5-城市场景1201)
  - [5.1 MAP OVERVIEW](#51-map-overview)
  - [5.2 Trajectory](#52-trajectory)
  - [5.3 RPE/APE](#53-rpeape)
- [6. 匝道 + 高架场景（12.02）](#6-匝道--高架场景1202)
  - [6.1 MAP OVERVIEW](#61-map-overview)
  - [6.2 Trajectory](#62-trajectory)
  - [6.3 RPE/APE](#63-rpeape)
- [7. 资源消耗](#7-资源消耗)

# 1. 待测试：

## 1.1 Lidar 里程计：

- [x] 1）基于LOAM框架；

- [x] 2）去地面；

- [x] 3）edge特征替换为beam和pillar；

- [x] 4）surf特征替换为facade和roof。

## 1.2 Lidar-IMU 里程计

- [x] 1）基于FAST LIO框架；

- [x] 2）去地面，运动补偿；

- [x] 3）使用beam，pillar，facade，roof特征点

# 2. 测试方法：

- [x] 以GNSS惯导数据作为Ground truth。

- [x] 两种场景：1）隧道。2）城市；3）匝道+高架路；

- [x] 两种方法：1）使用点云帧头尾两帧附近的IMU数据插值做运动补偿；2）使用点云帧各个点时间戳附近的IMU数据去畸变。

- [x] 去除地面和动态车辆。

- [x] 对比轨迹精度（APE/RPE）。

- [x] 比较建图效果。

# 3. 测试数据：

- [x] 匝道+高架rosbag，点云+imu数据。**没有记录IMU数据**

- [ ] 高架rosbag，点云+imu数据。**没有记录IMU数据**

- [x] 城市场景，十字路口。点云+imu数据。

- [x] 隧道rosbag，点云+imu数据。


# 4. 隧道场景（11.30）

<https://printeger.github.io/posts/Tunnel-Mapping/>

# 5. 城市场景（12.01）
## 5.1 MAP OVERVIEW

|  | MAP OVERVIEW |
|:-----:|:-----:|
| **LO** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/1.png) |
| **LIO** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/2.png) |
| **结论** | 1. 纯点云里程计的建图效果优于FAST LIO2建图效果。 |

## 5.2 Trajectory

|  | Trajectory | XYZ | RPY |
|:-----:|:-----:|:-----:|:-----:|
| **LO** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/3.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/4.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/5.png" width="250px" ></figure> |
| **LIO** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/6.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/7.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/8.png" width="250px" ></figure> |
| **结论** | 1. LIO轨迹偏离GT较大<br>z轴上由于有IMU的约束，<br>没有产生较大漂移| 2. 纯点云里程计的轨迹xy轴方向精度较高<br>z轴方向有较大漂移 | 3. 旋转上LIO效果更佳 |

## 5.3 RPE/APE

|  | RPE | RPE MAP | APE | APE MAP |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| **LO** | RPE:<br>max	0.305850<br>mean	0.066740<br>median  0.055630<br>min	0.002091<br>rmse	0.081193<br>sse	10.732192<br>std	0.046239 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/9.png" width="200px" ></figure> | APE: <br>max	11.688266<br>mean	7.092213<br>median    7.235089<br>min	0.000000<br>rmse	7.935915<br>sse	102592.372368<br>std	3.560795 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/10.png" width="200px" ></figure> |
| **LIO** | RPE：<br>max	0.532363<br>mean	0.070702<br>median  0.064539<br>min	0.001052<br>rmse	0.088508<br>sse	12.721818<br>std	0.053243 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/11.png" width="200px" ></figure> | APE：<br>max	20.612795<br>mean	10.268830<br>median    10.326422<br>min	0.077964<br>rmse	11.584797<br>sse	218087.226911<br>std	5.362709 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/12.png" width="200px" ></figure> |
| **结论** | 从相对轨迹误差(RPE)分析： | 1）纯雷达里程计的精度略优于改进的FAST LIO2<br>2）两者所估计的帧间相对运动精度在10cm内 | 从绝对轨迹误差(APE)分析： | 1）Lidar里程计的误差主要来源于z轴上的误差；<br>2）LIdar+IMU里程计的误差主要来源于水平方向的位置漂移。 |




# 6. 匝道 + 高架场景（12.02）
- [ ] 没有IMU信息

## 6.1 MAP OVERVIEW

|  | MAP OVERVIEW |
|:-----:|:-----:|
| **LO(有地面约束)** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/13.png)<br>_白色：惯导叠帧建图；绿色：地面/立面/柱面作为特征建图；红色：立面/柱面作为特征建图_ |
| **LO(无地面约束)** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/14.png)<br>_俯视图_ |
| **结论** | 1)纯Lidar里程计的建图z轴漂移较大。<br>2)加入地面约束能够有效约束z轴漂移。<br>3)水平方向上运动较准确。 |

## 6.2 Trajectory

|  | Trajectory | XYZ | RPY |
|:-----:|:-----:|:-----:|:-----:|
| **LO(有地面约束)** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/15.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/16.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/17.png" width="250px" ></figure> |
| **LO(无地面约束)** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/18.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/19.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/20.png" width="250px" ></figure> |
| **结论** | --------------------- | 加入地面约束能够有效约束z轴漂移 | --------------------- |

## 6.3 RPE/APE

|  | RPE | RPE MAP | APE | APE MAP |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| **LO(有地面约束)** | RPE:<br>max	0.969155<br>mean	0.118898<br>median    0.072171<br>min	0.009524<br>rmse	0.159581<br>sse	15.228651<br>std	0.106439 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/21.png" width="200px" ></figure> | APE: <br>max	16.723373<br>mean	8.182793<br>median   8.412561<br>min	0.000000<br>rmse	9.414864<br>sse	53095.156373<br>std	4.656347 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/22.png" width="200px" ></figure> |
| **LO(无地面约束)** | RPE:<br>max	0.680570<br>mean	0.129895<br>median 0.092094<br>min	0.006708<br>rmse	0.168222<br>sse	16.922574<br>std	0.106892 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/23.png" width="200px" ></figure> | APE: <br>max	36.819789<br>mean	15.548464<br>median   15.535765<br>min	0.000000<br>rmse	18.956620<br>sse	215252.705330<br>std	10.844294 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/24.png" width="200px" ></figure> |
| **结论** | 从相对轨迹误差(RPE)分析： | 帧间位姿平均误差大概在10cm左右。 | 从绝对轨迹误差(APE)分析： | 1）误差随着距离的增加累计。<br>2）误差主要出现在高程 |


# 7. 资源消耗

> FAST LIO：   CPU core：65%
>
 > RES：512 M
>
>  滤波：20ms
>
> LOAM：CPU core：300%
>
>  RES：500 M
>
>  优化：40 ms





