---
title: Lidar IMU Extrinsic Calibration Experiment
author: printeger
date: 2022-08-14 12:00:00 +0800
categories: [Project, Calibration]
tags: [LiDAR, IMU, Sensor, Calibration, Experiment, SLAM]
math: true
mermaid: true
---
- [1. Usage](#1-usage)
  - [1.1 Data Collect Procedure](#11-data-collect-procedure)
  - [1.2 Precision Indicators](#12-precision-indicators)
    - [1.2.1 RPE: relative pose error](#121-rpe-relative-pose-error)
    - [1.2.2 ATE: absolute trajectory error](#122-ate-absolute-trajectory-error)
  - [1.3 in \& out](#13-in--out)
- [2. Calibration Result](#2-calibration-result)
  - [2.0 Extrinsic Matrix](#20-extrinsic-matrix)
  - [2.1 Calib Data:(0655)](#21-calib-data0655)
    - [2.1.1 Traj](#211-traj)
    - [2.1.2 XYZ](#212-xyz)
    - [2.1.3 RPY](#213-rpy)
    - [2.1.4 APE](#214-ape)
    - [2.1.5 RPE](#215-rpe)
  - [2.2 Valid Data:(0657)](#22-valid-data0657)
    - [2.2.1 Traj](#221-traj)
    - [2.2.2 XYZ](#222-xyz)
    - [2.2.3 RPY](#223-rpy)
    - [2.2.4 APE](#224-ape)
    - [2.2.5 RPE](#225-rpe)
  - [2.3 Valid Data:(large sacle)](#23-valid-datalarge-sacle)
    - [2.3.1 Traj](#231-traj)
    - [2.3.2 XYZ](#232-xyz)
    - [2.3.3 RPY](#233-rpy)
    - [2.3.4 APE](#234-ape)
    - [2.3.5 RPE](#235-rpe)
  - [2.4 Valid Data:(7\_left\_turn\_speed\_25)](#24-valid-data7_left_turn_speed_25)
    - [2.4.1 Traj](#241-traj)
    - [2.4.2 XYZ](#242-xyz)
    - [2.4.3 RPY](#243-rpy)
    - [2.4.4 APE](#244-ape)
    - [2.4.5 RPE](#245-rpe)
  - [2.5 Valid Data:(8\_right\_turn\_speed\_20)](#25-valid-data8_right_turn_speed_20)
    - [2.5.1 Traj](#251-traj)
    - [2.5.2 XYZ](#252-xyz)
    - [2.5.3 RPY](#253-rpy)
    - [2.5.4 APE](#254-ape)
    - [2.5.5 RPE](#255-rpe)
  - [2.6 Valid Data:(9\_right\_turn\_speed\_40)](#26-valid-data9_right_turn_speed_40)
    - [2.6.1 Traj](#261-traj)
    - [2.6.2 XYZ](#262-xyz)
    - [2.6.3 RPY](#263-rpy)
    - [2.6.4 APE](#264-ape)
    - [2.6.5 RPE](#265-rpe)
  - [2.7 Valid Data:(10\_right\_turn\_speed\_40)](#27-valid-data10_right_turn_speed_40)
    - [2.7.1 Traj](#271-traj)
    - [2.7.2 XYZ](#272-xyz)
    - [2.7.3 RPY](#273-rpy)
    - [2.7.4 APE](#274-ape)
    - [2.7.5 RPE](#275-rpe)


# 1. Usage
## 1.1 Data Collect Procedure

More than three motions in different positions and orientations are required, so that the equation is full rank and can be solved linearly.

- [x] Therefore, the algorithm requires the trajectory needs to contain translations and rotations(like vehicle to travel in the shape of figure 8, as shown in Figure down below). 

- [x] The point cloud and GNSS+INS data of the trajectory need to be recorded simultaneously.

- [x] There should be rich features in the experimental environment, like buildings, and less moving objects, like moving vehicle, which will affect the result of Lidar odometry.

- [x] Do not test in an environment surrounded by tall buildings, this will affect the accuracy of GNSS.

- [x] Could record more than one sets of data for verification if necessary. 

![](pic/5/4.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/4.png)

## 1.2 Precision Indicators
> estimate pose: 
> 
>![](pic/5/17.png)
>![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/17.png)

> reference pose:
>
> ![](pic/5/18.png)
> ![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/18.png)

### 1.2.1 RPE: relative pose error
> i frame RPE:
> 
> ![](pic/5/19.png)
> ![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/19.png)

![](pic/5/20.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/20.png)

### 1.2.2 ATE: absolute trajectory error

![](pic/5/21.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/21.png)

![](pic/5/22.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/22.png)

![](pic/5/23.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/23.png)

## 1.3 in & out

Input:

```
// PointCloud
pcl::PointCloud<pointXYZ> point_in;
// IMU pose
std::vector<Eigen::Matrix4d> IMU_pose;
// PointCloud timestamps
std::vector<double> timestamps;
```
Output:

```
//Extrinsic Matrix: 
Eigen::Matrix4d calibratedTransformation;
/*
|  R T  |
|_ 0 1 _|
*/
```

# 2. Calibration Result
## 2.0 Extrinsic Matrix
```
[[0.9996387536200673, -0.01085860442221318, 0.02458562529040346, -1.697536587497878],
[-0.01096721316998588, -0.9999306683289505, 0.004287046827647254, -0.007142523421813133],
[0.02453736938227735, -0.004555133940977415, -0.99968853562426, -1.502642065472676],
[0, 0, 0, 1]]
```
## 2.1 Calib Data:(0655) 
### 2.1.1 Traj
![](pic/6/1.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/1.png)
### 2.1.2 XYZ
![](pic/6/2.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/2.png)
### 2.1.3 RPY
![](pic/6/3.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/3.png)
### 2.1.4 APE
```
max	0.307479
mean	0.123667
median	0.122317
min	0.009096
rmse	0.133519
sse	19.217960
std	0.050338
```
![](pic/6/4.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/4.png)
### 2.1.5 RPE
```
max	0.125233
mean	0.031746
median	0.027947
min	0.002622
rmse	0.038173
sse	1.569387
std	0.021198
```
![](pic/6/5.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/5.png)


## 2.2 Valid Data:(0657)
### 2.2.1 Traj
![](pic/6/6.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/6.png) 
### 2.2.2 XYZ
![](pic/6/7.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/7.png)
### 2.2.3 RPY
![](pic/6/8.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/8.png)
### 2.2.4 APE
```
max	0.352818
mean	0.108570
median	0.095848
min	0.006885
rmse	0.127275
sse	13.347926
std	0.066419
```
![](pic/6/9.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/9.png)
### 2.2.5 RPE
```
max	0.183633
mean	0.029524
median	0.026837
min	0.001288
rmse	0.034410
sse	0.974481
std	0.017675
```
![](pic/6/10.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/10.png)
## 2.3 Valid Data:(large sacle)
### 2.3.1 Traj
![](pic/6/11.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/11.png)
### 2.3.2 XYZ
![](pic/6/12.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/12.png)
### 2.3.3 RPY
![](pic/6/13.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/13.png)
### 2.3.4 APE
```
max	2.925505
mean	0.959726
median	0.750311
min	0.205600
rmse	1.168728
sse	4457.011678
std	0.666971
``` 
![](pic/6/14.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/14.png)
### 2.3.5 RPE
```
max	0.372968
mean	0.095626
median	0.073501
min	0.001336
rmse	0.127043
sse	52.648051
std	0.083639
```
![](pic/6/15.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/15.png)
## 2.4 Valid Data:(7_left_turn_speed_25)
### 2.4.1 Traj
![](pic/6/16.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/16.png)
### 2.4.2 XYZ
![](pic/6/17.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/17.png) 
### 2.4.3 RPY
![](pic/6/18.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/18.png)
### 2.4.4 APE
```
max	0.906424
mean	0.433745
median	0.458177
min	0.032235
rmse	0.482597
sse	44.483811
std	0.211576
```
![](pic/6/19.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/19.png)
### 2.4.5 RPE
```
max	0.122539
mean	0.059356
median	0.059501
min	0.006692
rmse	0.065355
sse	0.811532
std	0.027350
```
![](pic/6/20.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/20.png)
## 2.5 Valid Data:(8_right_turn_speed_20)
### 2.5.1 Traj
![](pic/6/21.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/21.png)
### 2.5.2 XYZ
![](pic/6/22.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/22.png)
### 2.5.3 RPY
![](pic/6/23.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/23.png)
### 2.5.4 APE
```
max	3.209363
mean	0.748066
median	0.543427
min	0.010104
rmse	1.007491
sse	163.421082
std	0.674860
```
![](pic/6/24.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/24.png)
### 2.5.5 RPE
```
max	1.053835
mean	0.117616
median	0.071761
min	0.011882
rmse	0.194937
sse	6.080060
std	0.155457
```
![](pic/6/25.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/25.png)
## 2.6 Valid Data:(9_right_turn_speed_40)
### 2.6.1 Traj
![](pic/6/26.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/26.png)
### 2.6.2 XYZ
![](pic/6/27.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/27.png)
### 2.6.3 RPY
![](pic/6/28.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/28.png)
### 2.6.4 APE
```
max	1.115413
mean	0.527681
median	0.531853
min	0.126005
rmse	0.557352
sse	31.685443
std	0.179429
``` 
![](pic/6/29.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/29.png)
### 2.6.5 RPE
```
max	0.443761
mean	0.118309
median	0.098930
min	0.012560
rmse	0.149402
sse	2.254411
std	0.091235
```
![](pic/6/30.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/30.png)
## 2.7 Valid Data:(10_right_turn_speed_40)
### 2.7.1 Traj
![](pic/6/31.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/31.png)
### 2.7.2 XYZ
![](pic/6/32.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/32.png)
### 2.7.3 RPY
![](pic/6/33.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/33.png)
### 2.7.4 APE
```
max	1.584476
mean	0.527325
median	0.449033
min	0.076434
rmse	0.602559
sse	52.646241
std	0.291557
```
![](pic/6/34.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/34.png)
### 2.7.5 RPE
```
max	1.256550
mean	0.229395
median	0.141439
min	0.008515
rmse	0.368821
sse	19.588214
std	0.288803
```
![](pic/6/35.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/6/35.png)



