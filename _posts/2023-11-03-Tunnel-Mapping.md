---
title: Tunnel Mapping
author: printeger
date: 2022-11-03 12:00:00 +0800
categories: [Project, SLAM]
tags: [LiDAR, SLAM, Experiment]
math: true
mermaid: true
---

- [0. 扬州瘦西湖隧道](#0-扬州瘦西湖隧道)
- [1. Integrated navigation with GNSS \& IMU mapping](#1-integrated-navigation-with-gnss--imu-mapping)
  - [1.1 建图效果](#11-建图效果)
  - [1.2 轨迹](#12-轨迹)
- [2. Feature based SLAM](#2-feature-based-slam)
  - [2.1 建图效果](#21-建图效果)
  - [2.2 轨迹对比](#22-轨迹对比)
- [3. Lidar-IMU SLAM](#3-lidar-imu-slam)
  - [3.1 使用全部点云匹配：](#31-使用全部点云匹配)
    - [建图效果](#建图效果)
    - [轨迹对比](#轨迹对比)
  - [3.2 使用特征匹配：](#32-使用特征匹配)
    - [建图效果](#建图效果-1)
    - [轨迹对比](#轨迹对比-1)
- [4. FEATURE EXTRACTION（11.25）](#4-feature-extraction1125)
  - [4.1 FEATURES](#41-features)
    - [FLOAM](#floam)
    - [MULLS](#mulls)
    - [zheng](#zheng)
    - [改进特征](#改进特征)
  - [4.2 特征匹配效果测试](#42-特征匹配效果测试)
    - [facade](#facade)
    - [beam](#beam)
    - [pillar](#pillar)
    - [vertex](#vertex)
    - [ground](#ground)
- [5. 改进特征建图（11.30）](#5-改进特征建图1130)
  - [5.1 Lidar + IMU 融合：](#51-lidar--imu-融合)
  - [5.2 OVERVIEW](#52-overview)
  - [5.3 分段建图](#53-分段建图)

# 0. 扬州瘦西湖隧道

- [ ] 全长4.4km

- [ ] 共设有12组逃生通道

# 1. Integrated navigation with GNSS & IMU mapping

## 1.1 建图效果

- [ ] 根据ins位姿映射到起点

- [ ] 动态物体未去除

- [ ] 帧与帧之间存在误差，对不齐

![](pic/9/1.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/1.png)

![](pic/9/2.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/2.png)

![](pic/9/3.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/3.png)

## 1.2 轨迹

- [ ] 轨迹长度符合真实情况，用作后续比较的**ground truth**

![](pic/9/4.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/4.png)

![](pic/9/5.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/5.png)

![](pic/9/6.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/6.png)

# 2. Feature based SLAM

## 2.1 建图效果

- [ ] 动态物体未去除，同行车辆被当作特征建图。

![](pic/9/7.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/7.png)

## 2.2 轨迹对比

- [ ] 700m vs 3200m

- [ ] 进入隧道后一段距离可以跟踪建图，重复特征较多时，难以获得有效特征匹配，无法得到有效pose。
<center>
<figure>

<img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/8.png">
<img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/9.png">
<img src="https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/10.png">

</figure>
</center>

![](pic/9/8.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/8.png)

![](pic/9/9.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/9.png)

![](pic/9/10.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/10.png)



# 3. Lidar-IMU SLAM

## 3.1 使用全部点云匹配：

### 建图效果

![](pic/9/11.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/11.png)

### 轨迹对比

1200m vs 3200m

在1）进出隧道；2）有转弯，的地方能够估计出自车运动，轨迹可以反映基本形状。

在长直道难以估计出位姿

![](pic/9/12.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/12.png)

![](pic/9/13.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/13.png)

![](pic/9/14.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/14.png)

## 3.2 使用特征匹配：

- [ ] 效果不如全局匹配

### 建图效果



### 轨迹对比

- [ ] 1100m vs 3200m

![](pic/9/15.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/15.png)

![](pic/9/16.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/16.png)

![](pic/9/17.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/17.png)







# 4. FEATURE EXTRACTION（11.25）

> 有用的特征：边缘曲率，梁，扫描线

## 4.1 FEATURES

### FLOAM

- [ ] LOAM类提取方法。

![](pic/9/18.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/18.png)
### MULLS

- [ ] 地面：白色，提取效果不错，提取距离大概在60m

- [ ] 柱面：隧道内无明显柱面特征

- [ ] 立面：黄色。主要分布于两侧墙面，前车后立面也会被提取。

- [ ] 梁：红色。点云扫描边缘，车道与墙面交汇处，以及上方红绿灯等横梁。

- [ ] 角点：蓝色。分布于两侧墙面，点云稀疏处。

- [ ] find ground: 5.29322ms

- [ ] find unground: 94.9289ms

- [ ] Pillar: [906 / 0] Beam: [1482 / 0] Facade: [365 / 0] Roof: [16 / 0] Vertex: [1402]

- [ ] 匹配效果见下节。

![](pic/9/19.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/19.png)

### zheng

- [ ] 平面点聚类，记录特征中心。

- [ ] 地面与地面上的点被去除。

- [ ] full: 48.6759ms

- [ ] center: 55.2746ms

![](pic/9/20.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/20.png)

![](pic/9/21.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/21.png)

### 改进特征



- [x] 最远可以探测到110～130m处的梁特征。

- [x] beam约束前后运动，facade约束航向运动。

特征

![](pic/9/22.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/22.png)

匹配结果

![](pic/9/23.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/23.png)

![](pic/9/24.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/24.png)

> TRANSLATION:  1.8917817372525866 -0.3589180073315425 -0.1240118893990635
> ROTATION:  -0.582874110838635 -0.17952369378155364 0.8506732502603874
>
> GT: 1.60161103005521 -0.03901633154600859 0.003720984378560388

## 4.2 特征匹配效果测试

### facade


> ROTATION:  0.10026415229906012 -0.037789288636643104 0.028922883060923277
> TRANSFORM:  -0.0022341084174282697 -0.020622324293664538 -0.009246697937472887

![](pic/9/25.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/25.png)
### beam

> ROTATION:  -0.14544158527269985 -0.15685605495471888 -0.7940208012581766
> TRANSFORM:  -1.497877384899562 0.2293188223521817 -0.2220930518225458

![](pic/9/26.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/26.png)
### pillar

> ROTATION:  -6.135291476676384 2.7929696079187045 0.3720736798573453
> TRANSFORM:  -2.772918442653669 -0.15609526647733912 1.1209641884333805

![](pic/9/27.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/27.png)
### vertex

> ROTATION:  0.08136797872529496 -0.32988581367588 -0.4571218152110009
> TRANSFORM:  -0.3469783235783132 0.05848230771925661 -0.12645609754115078

![](pic/9/28.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/28.png)
### ground

> ROTATION:  0.3689452864417122 -0.06710693876083082 -0.1895471521748845
> TRANSFORM:  -0.020153441095042326 0.26792033040768265 -0.013160392521254413

![](pic/9/29.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/29.png)

# 5. 改进特征建图（11.30）

## 5.1 Lidar + IMU 融合：

| 3222.735m | 特征：beam+pillar+facade | 特征：beam+pillar |
|:-----:|:-----:|:-----:|
| 使用IMU做运动补偿---YES | FAST_LIO_mcon_beam_pillar_facade | FAST_LIO_mcon_beam_pillar |
| 使用IMU做运动补偿---NO | FAST_LIO_mcoff_beam_pillar_facade | FAST_LIO_mcoff_beam_pillar |


## 5.2 OVERVIEW

- [ ] 新特征对位姿的效果有较大提升，推断出的隧道长度2.83km大概接近隧道长度3.22km。

```
name:	GROUND TRUTH
infos:	2008 poses, 3222.735m path length

name:	FAST_LIO_mcoff_beam_pillar_facade
infos:	2002 poses, 2784.669m path length

name:	FAST_LIO_mcon_beam_pillar_facade
infos:	2002 poses, 2834.529m path length

name:	FAST_LIO_mcoff_beam_pillar
infos:	2003 poses, 2811.946m path length

name:	FAST_LIO_mcon_beam_pillar
infos:	2003 poses, 2833.535m path length
```

- [ ] 隧道中后端开始偏离原始轨迹，测试分段估计？

- [ ] 高度上估计误差较大，不使用facade特征和运动补偿时最接近GT高度。

- [ ] ROLL角变化趋势偏离GT较大，末段偏离5度左右。PITCH和YAW角基本吻合GT变化趋势。

![](pic/9/30.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/30.png)

![](pic/9/31.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/31.png)

![](pic/9/32.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/32.png)

- [ ] 运动补偿能带来相对更准确的相对位姿。

- [ ] 从APE看，整体轨迹在后半段偏离较大，仍是加了运动补偿的效果较好。

|  | FAST_LIO_mcon_beam_pillar_facade | FAST_LIO_mcoff_beam_pillar_facade | FAST_LIO_mcon_beam_pillar | FAST_LIO_mcoff_beam_pillar|
|:-----:|:-----:|:-----:|:-----:|:-----:|
| RPE | rmse	0.310271 | rmse	0.340221 | rmse	0.317437 | rmse	0.326531 |
| APE | rmse	186.160158 | rmse	210.094900 | rmse	186.009823 | rmse	195.803037 |

## 5.3 分段建图

将隧道分为四段: 
- [ ] 隧道外到隧道入口段
- [ ] 隧道入口至并道段
- [ ] 并道至长直段（700 帧）
- [ ] 长直段至隧道出口（721帧）

![](pic/9/33.png)
![](https://github.com/Printeger/printeger.github.io/raw/master/_posts/pic/9/33.png)

第3/4段开始出现较大偏差。
