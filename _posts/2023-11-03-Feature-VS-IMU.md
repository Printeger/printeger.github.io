---
title: Feature VS IMU
author: printeger
date: 2022-11-03 12:00:00 +0800
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
    - [2.3.2 使用IMU去畸变/IMU位姿叠帧建图](#232-使用imu去畸变imu位姿叠帧建图)


# 0_1. 分析数据：


# 0_2. 步骤：

- [x] 单帧：

1.提取地面，提取特征

2.单帧与静态帧匹配，

3.分别按匹配结果与IMU结果，计算单帧点云与静止帧中最近点距离，统计在不同距离范围中点的占比。

4.按照距离分配intensity

- [x] 建图：

1.使用里程计位姿去畸变; 

2.里程计位姿叠帧建图;

3.IMU位姿去畸变；

4.IMU叠帧建图；

5.与静止场景比较特征重合程度；

6.计算静止点云与地图中最近的点的距离，统计在不同距离范围中点的占比。

7.按照距离分配intensity

# 1. 单帧ICP(scan-to-scan)

- [x] 第0帧（静止帧）与第251帧（动态帧，已去畸变）对比

## 1.1 位置/姿态变换结果对比

|  | IMU测量 | 里程计结果 | 所有点匹配 | 去地面点匹配 |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| **pos.x** | 11.9864419965346 | 12.113 | 12.2315209393 | 12.19829592444 |
| **pos.y** | 0.35545271630706 | 0.279951 | 0.20742369269 | 0.201242980913 |
| **pos.z** | -0.027396272823 | 0.229392 | 0.03955666560 | 0.150457801652 |
| **roll** | 0.12564565735257   | 0.1990477499999 | 0.26103444641 | 0.075069163511 |
| **pitch** | -0.01561664334409 | 0.0350390808002 | -0.38558692648 | -0.20547417157 |
| **yaw** | 1.713945651730975 | 1.7661607749408 | 1.92228541454 | 1.942115501283 |


## 1.2 使用不同特征匹配结果：
- **结论：**

- [x] 地面点可以有效约束z轴漂移。

- [x] 去除地面点匹配的精度更高。

- [x] 单独的柱面/立面等特征无法完全约束三个方向的运动。


| 原始点云<br>第0帧与第251帧 | 惯导测量结果 | LiDAR里程计测量结果 |
|:-----:|:-----:|:-----:|
| ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/1.png) | **TRANSLATION**:  <br>11.98644199653465 <br>0.35545271630706643 <br>-0.027396272823478673<br>**ROTATION**:<br>0.12564565735257952<br>-0.015616643344094927<br>1.7139456517309755 |**TRANSLATION**:<br>12.113<br>0.279951<br>0.229392<br>**ROTATION**:<br>0.19904774999987535<br>0.03503908080022266 <br>1.766160774940752 |


| 采用特征 | 匹配效果 | 匹配结果 |
|:-----:|:-----:|:-----|
| **所有点云** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/2.png) | **TRANSLATION**:  <br>12.231520939336493 <br>0.2074236926917551 <br>0.03955666560427022<br>**ROTATION**:  <br>0.26103444641217954 <br>-0.38558692648123116 <br>1.9222854145442057 |
| **nonground** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/3.png) | **TRANSLATION**:  <br>12.198295924444954 <br>0.20124298091307577 <br>0.1504578016521565<br>**ROTATION**:  <br>0.07506916351143701 <br>-0.20547417157562423 <br>1.9421155012833935 |
| **ground** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/4.png) | **TRANSLATION**:  <br>-0.45032801957550905 <br>-1.539946635341638 <br>-0.04409875650376381<br>**ROTATION**:  <br>0.061396539662778 <br>0.0393528844241862 <br>2.687272808286958 |
| **facade** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/5.png) | **TRANSLATION**:  <br>0.18602666396360037 <br>0.003922289244317075 <br>-0.659840212760906<br>**ROTATION**:  <br>0.9523135428663062 <br>-0.7056622717456797 <br>2.622647650589112 |
| **pillar** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/6.png) | **TRANSLATION**:  <br>1.9201082213595009 <br>1.6873773196469384 <br>-0.018532779692659894<br>**ROTATION**:  <br>-1.5216757902856088 <br>-0.8649583504104713 <br>-0.7739914823982094 |
| **beam** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/7.png) | **TRANSLATION**:  <br>-0.2902069118127943 <br>1.1793059954475964 <br>-0.25265109924299545<br>**ROTATION**:  <br>-2.5767941530217193 <br>0.09564350376227218 <br>0.563153967273353 |
| **vertex** | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/8.png) | **TRANSLATION**:  <br>1.4010472774746574 <br>-1.084962950016715 <br>-0.733484141649384<br>**ROTATION**:  <br>0.47538640627435874 <br>-1.475592489579815 <br>4.088730099925485 |


## 1.3 单帧点云与静止帧中点距分析：
**实验方法：**

> 分别按匹配结果与IMU结果，计算单帧点云与静止帧中最近点距离，统计在不同距离范围中点的占比。以此来反映匹配精度。

**结论：**
- [x] 去除地面点后进行匹配，整体点云，以及非地面点云重合程度更好。

- [x] 里程计得到的变换误差大于0.5m的点占比最少；地面点云重合程度相对更高。

|  | IMU测量矩阵 | 里程计测量矩阵 | 所有点ICP | 非地面点ICP |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| 全部点云<br>81836 points | dist < 0.1: 9463 (11.56%)  <br>[0.1, 0.2): 20743 (25.35%) <br>[0.2, 0.3): 28139 (34.38%)  <br>[0.3, 0.4): 8918 (10.90%) <br>[0.4, 0.5): 4516 (5.52%) <br>[0.5, ~ ~): 10057 (12.29%) | dist < 0.1: 26723 (32.65%)  <br>[0.1, 0.2): 28379 (34.68%)  <br>[0.2, 0.3): 9651 (11.79%)  <br>[0.3, 0.4): 5171 (6.32%)  <br>[0.4, 0.5): 3300 (4.03%) <br>**[0.5, ~ ~): 8612 (10.52%)** | dist < 0.1: 32270 (39.43%)  <br>[0.1, 0.2): 20806 (25.42%)  <br>[0.2, 0.3): 9305 (11.37%)  <br>[0.3, 0.4): 5685 (6.95%)  <br>[0.4, 0.5): 3771 (4.61%)  <br>[0.5, ~ ~): 9999 (12.22%) | dist **< 0.1: 32990 (40.31%)**  <br>[0.1, 0.2): 20168 (24.64%)  <br>[0.2, 0.3): 9409 (11.50%)  <br>[0.3, 0.4): 5732 (7.00%)  <br>[0.4, 0.5): 3826 (4.68%)  <br>[0.5, ~ ~): 9711 (11.87%) |
| 地面<br>9936 points | dist < 0.1: 9 (0.09%)<br>[0.1, 0.2): 574 (5.78%)<br>[0.2, 0.3): 3940 (39.65%)<br>[0.3, 0.4): 1586 (15.96%)<br>[0.4, 0.5): 1061 (10.68%)<br>[0.5, ~ ~): 2766 (27.84%) | dist **< 0.1: 1899 (19.11%)**<br>[0.1, 0.2): 2128 (21.42%)<br>[0.2, 0.3): 1551 (15.61%)<br>[0.3, 0.4): 1055 (10.62%)<br>[0.4, 0.5): 822 (8.27%)<br>**[0.5, ~ ~): 2481 (24.97%)** | dist < 0.1: 1322 (13.31%)<br>[0.1, 0.2): 2619 (26.36%)<br>[0.2, 0.3): 1508 (15.18%)<br>[0.3, 0.4): 1010 (10.17%)<br>[0.4, 0.5): 805 (8.10%)<br>[0.5, ~ ~): 2672 (26.89%) | dist < 0.1: 1774 (17.85%)<br>[0.1, 0.2): 2207 (22.21%)<br>[0.2, 0.3): 1496 (15.06%)<br>[0.3, 0.4): 1057 (10.64%)<br>[0.4, 0.5): 775 (7.80%)<br>[0.5, ~ ~): 2627 (26.44%) |
| 去地面点<br>19897 points | dist < 0.1: 1467 (7.37%)<br>[0.1, 0.2): 4889 (24.57%)<br>[0.2, 0.3): 5932 (29.81%)<br>[0.3, 0.4): 3178 (15.97%)<br>[0.4, 0.5): 1393 (7.00%)<br>[0.5, ~ ~): 3038 (15.27%) | dist < 0.1: 3850 (19.35%)<br>[0.1, 0.2): 7387 (37.13%)<br>[0.2, 0.3): 3342 (16.80%)<br>[0.3, 0.4): 1768 (8.89%)<br>[0.4, 0.5): 1004 (5.05%)<br>**[0.5, ~ ~): 2546 (12.80%)** | dist < 0.1: 5437 (27.33%)<br>[0.1, 0.2): 5784 (29.07%)<br>[0.2, 0.3): 2886 (14.50%)<br>[0.3, 0.4): 1738 (8.73%)<br>[0.4, 0.5): 1108 (5.57%)<br>[0.5, ~ ~): 2944 (14.80%) | dist **< 0.1: 5453 (27.41%)**<br>[0.1, 0.2): 5817 (29.24%)<br>[0.2, 0.3): 2917 (14.66%)<br>[0.3, 0.4): 1737 (8.73%)<br>[0.4, 0.5): 1128 (5.67%)<br>[0.5, ~ ~): 2845 (14.30%) |


# 2. 里程计全局建图（scan-to-map）

## 2.1 轨迹：

- [x] 点云匹配的到的位姿在局部更加精确，体现在柱面粗细、墙面厚度拟合的精度，在较大范围内z轴上漂移较大。

<center>
<figure>
<img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/9.png"  width="250px" >
<img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/10.png"  width="250px" >
</figure>
</center>

## 2.2 量化分析：

- [x] 特征匹配建图的点云与静止点云的匹配精度较IMU + GPS的精度更好。

- [x] 匹配后点距大于0.5m的点主要为未去除的动态物体（车辆/行人）。

- [x] 高度越高，匹配精度越差。

### 2.2.1 TEST SCENCE 1

| USING ODOM | USING IMU |
|:-----:|:-----:|
| ODOM MAP 1,000,000 points<br>STATIC TOTAL: 167635 points<br> dist < 0.1: 57218 (34.13249023175351%)<br> [0.1,0.2): 64855 (38.68822143347153%)<br> [0.2, 0.3): 21072 (12.570167327825335%)<br> [0.3, 0.4): 9238 (5.510782354520238%)<br> [0.4, 0.5): 5758 (3.4348435589226596%)<br> [0.5, ~ ~): 9494 (5.663495093506725%) | IMU MAP 1,000,000 points<br>STATIC TOTAL: 167635 points<br> dist < 0.1: 24193 (14.43195036835983%)<br> [0.1, 0.2): 77132 (46.01187102931965%)<br> [0.2, 0.3): 40690 (24.27297402093835%)<br> [0.3, 0.4): 11747 (7.0074865034151586%)<br> [0.4, 0.5): 4409 (2.630119008560265%)<br> [0.5, ~ ~): 9464 (5.645599069406747%) |
| ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/11.png) | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/12.png) |
| _ODOM DISTANCE 0~1m_ | _IMU DISTANCE MAP 0~1m_ |
| ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/13.png) | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/14.png) |
| _ODOM DISTANCE MAP 0～0.5m_ | _IMU DISTANCE MAP 0~0.5m_ |

### 2.2.2 TEST SCENCE 2

| USING ODOM | USING IMU |
|:-----:|:-----:|
| ODOM MAP 1,000,000 points<br>STATIC TOTAL: 124486 points<br> dist < 0.1: 109772 (88.1801969699404%)<br> [0.1, 0.2): 13553 (10.887168034959755%)<br> [0.2, 0.3): 144 (0.11567565830695821%)<br> [0.3, 0.4): 95 (0.07631380235528494%)<br> [0.4, 0.5): 91 (0.07310058962453608%)<br> [0.5, ~ ~): 831 (0.6675449448130714%) | IMU MAP 1,000,000 points<br>STATIC TOTAL: 124486 points<br> dist < 0.1: 83008 (66.68059058849991%)<br> [0.1, 0.2): 40045 (32.168275950709315%)<br> [0.2, 0.3): 397 (0.3189113635268223%)<br> [0.3, 0.4): 101 (0.08113362145140819%)<br> [0.4, 0.5): 94 (0.07551049917259772%)<br> [0.5, ~ ~): 841 (0.6755779766399435%)  |
| ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/15.png) | ![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/16.png) |
| _ODOM DISTANCE 0~0.5m_ | _IMU DISTANCE MAP 0~0.5m_ |


## 2.3 点云展示：

### 2.3.1 使用里程计位姿去畸变/里程计位姿叠帧建图

|  | overview | pillar | facade |
|:-----:|:-----:|:-----:|:-----:|
| **SCENE 1** | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/17.png" width="200px" ></figure></center> | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/18.png" width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/19.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/20.png"  width="200px" ></figure></center> | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/21.png" width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/22.png"  width="200px" ></figure></center> |
|  |  | 地图柱面拟合程度较好 |  |
| **SCENE 2** | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/23.png" width="200px" ></figure></center> | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/24.png" width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/25.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/26.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/27.png"  width="200px" ></figure></center> | <center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/28.png" width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/29.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/30.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/31.png"  width="200px" ></figure></center> |
|  |  | 近处柱面不贴合，远处贴合程度较好。 |  |
### 2.3.2 使用IMU去畸变/IMU位姿叠帧建图

|  | overview | pillar | facade |
|:-----:|:-----:|:-----:|:-----:|
| **SCENE 1** |  |  |  |
|  |  |  |  |
| **SCENE 2** |  |  |  |
|  |  |  |  |




地图柱面拟合程度较好

灯柱部分点云重合程度差变粗


右侧墙面变厚明显。




<center><figure><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/28.png" width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/29.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/30.png"  width="200px" ><br><img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/10/31.png"  width="200px" ></figure></center>

