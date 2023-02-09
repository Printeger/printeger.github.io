---
title: LO/LIO Performence Experiment In Different Road Scenes
author: printeger
date: 2022-11-30 12:00:00 +0800
categories: [Project, SLAM]
tags: [LiDAR, SLAM, Experiment]
math: true
mermaid: true
---

- [0\_1. 分析数据：](#0_1-分析数据)
- [0\_2. 步骤：](#0_2-步骤)
- [1. 单帧ICP(scan-to-scan)](#1-单帧icpscan-to-scan)
  - [1.1 位置/姿态变换结果对比](#11-位置姿态变换结果对比)
  - [1.2 使用不同特征匹配结果：](#12-使用不同特征匹配结果)
  - [1.3 单帧点云与静止帧中点距分析：](#13-单帧点云与静止帧中点距分析)
- [2. 里程计全局建图（scan-to-map）](#2-里程计全局建图scan-to-map)
  - [2.1 轨迹：](#21-轨迹)
  - [2.2 量化分析：](#22-量化分析)
    - [2.2.1 TEST SCENCE 1](#221-test-scence-1)
    - [2.2.2 TEST SCENCE 2](#222-test-scence-2)
  - [2.3 点云展示：](#23-点云展示)
    - [2.3.1 使用里程计位姿去畸变/里程计位姿叠帧建图](#231-使用里程计位姿去畸变里程计位姿叠帧建图)
      - [SCENE 1](#scene-1)
      - [SCENE 2](#scene-2)
    - [2.3.2 使用IMU去畸变/IMU位姿叠帧建图](#232-使用imu去畸变imu位姿叠帧建图)
      - [SCENE 1](#scene-1-1)
      - [SCENE 2](#scene-2-1)

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

|  | MAP OVERVIEW |
|:-----:|:-----:|
| **LO** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/1.png) |
| **LIO** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/2.png) |
| **结论** | 1. 纯点云里程计的建图效果优于FAST LIO2建图效果。 |


|  | Trajectory | XYZ | RPY |
|:-----:|:-----:|:-----:|:-----:|
| **LO** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/3.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/4.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/5.png" width="250px" ></figure> |
| **LIO** | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/6.png" width="150px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/7.png" width="200px" ></figure> | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/8.png" width="250px" ></figure> |
| **结论** | 1. FAST LIO2的轨迹偏离GROUND TRUTH较大<br>z轴上由于有IMU的约束，没有产生较大漂移| 2. 纯点云里程计的轨迹xy轴方向精度较高<br>z轴方向有较大漂移 | 3. 旋转上LIO效果更佳 |

|  | RPE | RPE MAP | APE | APE MAP |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| **LO** | RPE:<br>max	0.305850<br>mean	0.066740<br>median  0.055630<br>min	0.002091<br>rmse	0.081193<br>sse	10.732192<br>std	0.046239 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/9.png" width="200px" ></figure> | APE: <br>max	11.688266<br>mean	7.092213<br>median    7.235089<br>min	0.000000<br>rmse	7.935915<br>sse	102592.372368<br>std	3.560795 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/10.png" width="200px" ></figure> |
| **LIO** | RPE：<br>max	0.532363<br>mean	0.070702<br>median  0.064539<br>min	0.001052<br>rmse	0.088508<br>sse	12.721818<br>std	0.053243 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/11.png" width="200px" ></figure> | APE：<br>max	20.612795<br>mean	10.268830<br>median    10.326422<br>min	0.077964<br>rmse	11.584797<br>sse	218087.226911<br>std	5.362709 | <figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/11/12.png" width="200px" ></figure> |
| **结论** | 从相对轨迹误差(RPE)分析： | 1）纯雷达里程计的精度略优于改进的FAST LIO2<br>2）两者所估计的帧间相对运动精度在10cm内 | 从绝对轨迹误差(APE)分析： | 1）Lidar里程计的误差主要来源于z轴上的误差；<br>2）LIdar+IMU里程计的误差主要来源于水平方向的位置漂移。 |




# 6. 匝道 + 高架场景（12.02）