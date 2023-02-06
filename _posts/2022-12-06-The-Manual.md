---
title: The Manual
author: printeger
date: 2022-12-06 12:00:00 +0800
categories: [Lookup]
tags: [Dict]
math: true
mermaid: true
# image:
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---
- [A](#a)
  - [APOLLO](#apollo)
    - [play .record](#play-record)
    - [channle 监控](#channle-监控)
    - [sdk/repo download](#sdkrepo-download)
    - [启动数据集](#启动数据集)
    - [点云录制](#点云录制)
    - [orders](#orders)
- [B](#b)
- [C](#c)
  - [CUDA](#cuda)
    - [pytorch 对应 cuda版本](#pytorch-对应-cuda版本)
    - [CUDA](#cuda-1)
    - [cudnn .tar  install](#cudnn-tar--install)
- [D](#d)
  - [Docker](#docker)
    - [docker 可视化](#docker-可视化)
    - [Cannot access GPU from within container. Please install latest Docker and NVIDIA Container Toolkit as described by:](#cannot-access-gpu-from-within-container-please-install-latest-docker-and-nvidia-container-toolkit-as-described-by)
    - [报错Failed to connect. Is Docker running](#报错failed-to-connect-is-docker-running)
    - [docker  container 查看/启动/进入](#docker--container-查看启动进入)
    - [docker group](#docker-group)
    - [docker images / container](#docker-images--container)
    - [docker 拷贝本地文件到容器/容器到本地文件](#docker-拷贝本地文件到容器容器到本地文件)
- [E](#e)
  - [ERROR](#error)
  - [EVO](#evo)
- [F](#f)
- [G](#g)
  - [GDB](#gdb)
  - [GIT](#git)
    - [从bitbucket下载：](#从bitbucket下载)
- [H](#h)
- [I](#i)
  - [INNOVUSION](#innovusion)
    - [falcon-ros:](#falcon-ros)
- [J](#j)
- [K](#k)
- [L](#l)
- [M](#m)
- [N](#n)
- [O](#o)
- [P](#p)
  - [python](#python)
- [Q](#q)
- [R](#r)
  - [ROS](#ros)
    - [fixed frame trans](#fixed-frame-trans)
    - [Rosbag to pcd/txt:](#rosbag-to-pcdtxt)
    - [查看frame\_id](#查看frame_id)
- [S](#s)
- [T](#t)
  - [torch](#torch)
- [U](#u)
- [V](#v)
- [W](#w)
- [X](#x)
- [Y](#y)
- [Z](#z)

# A
## APOLLO
### play .record

	cyber_recorder play -f sensor_rgb.record -loop 
	cyber_visualizer 
### channle 监控
	cyber_monitor -c ChannelName

### sdk/repo download
    cd omnisense && ./tools/build.bash init all

### 启动数据集

		source /apollo/cyber/setup.bash 
		1 改launch/conf/driver_01_debug.pb.txt    --------------------data route
		2 改launch/dag/driver_single_debug.dag   -------------------config_file_path
		3 改launch/intergration_single_lidar_debug.launch  ------------master    .dag_path
		4 cyber_launch start modules/omnisense/launch/integration_single_lidar_debug.launch
		5 http://viewer.innovusion.info/v60/?url=127.0.0.1&port=8003/stream&autoplay=1.0

### 点云录制

		1. get_pcd/01.get_pcd.pb.txt   ----------channel1: dynamic_points  ----- channel2: static_points     channel3: -----enable_channel1_get_pcd:true
		2. .dag修改路径
### orders

	root@in-dev-docker:/apollo# ./apollo.sh build_opt omnisense cyber
	
	root@in-dev-docker:/apollo/modules/omnisense# git branch

* master

  root@in-dev-docker:/apollo/modules/omnisense# git checkout dev
  Branch 'dev' set up to track remote branch 'dev' from 'origin'.
  Switched to a new branch 'dev'
  root@in-dev-docker:/apollo/modules/omnisense# cd -
  /apollo

  root@in-dev-docker:/apollo# ./modules/omnisense/tools/build.bash build all

  xng@xng:~/CODE/OmniSense2.0/apollo$ ./docker/scripts/dev_into.sh

# B

# C
## CUDA
### pytorch 对应 cuda版本

	import torch
	print(torch.version.cuda)

### CUDA

	export PATH=/usr/local/cuda/bin:$PATH
	export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

### cudnn .tar  install

	sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
	sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
	sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
# D
## Docker
### docker 可视化

```
// RVIZ cannot display
docker run --add-host=demo:172.16.115.75 -it --name osrf_melodic_full --net host --add-host $(hostname):127.0.0.1 --gpus all -v /tmp/.x11-unix:/tmp/.x11-unix -e DISPLAY=unix$DISPLAY -e GDK_SCALE -e GDK_DPI_SCALE -v /home/xng/CODE/osrf_melodic_full/:/home osrf/ros:melodic-desktop-full /bin/bash
```

``` 
docker run –name ContainerName –gpus all –network host –env=DISPLAY –env=NVIDIA_DRIVER_CAPABILITIES=all -env=NVIDIA-VISIBLE_DEVICES=all –env=QT_X11_NO_MITSHM=1 -v /tmp/.X11-unix:/tmp/.X11-unix:rw –runtime=nvidia –privileged -it DockerImage
```
------
### Cannot access GPU from within container. Please install latest Docker and NVIDIA Container Toolkit as described by: 

- https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

### 报错Failed to connect. Is Docker running

	Error: connect EACCES /var/run/docker.sock
	sudo groupadd docker          #添加docker用户组
	sudo gpasswd -a $USER docker  #将当前用户添加至docker用户组
	newgrp docker                 #更新docker用户组
	在终端打开vsode
	code

### docker  container 查看/启动/进入

     	docker container ls --all 
    	docker start apollo_dev_root
    	docker ps 
    	sudo docker exec -it ae76016acd18 /bin/bash
    
    	source /apollo/cyber/setup.bash 

### docker group

	sudo groupadd docker
	sudo usermod -aG docker $USER
	newgrp docker

### docker images / container

	docker pull
	docker run -it {name}:
	docker exec -it {ID} /bin/bash

###  docker 拷贝本地文件到容器/容器到本地文件

	docker cp 你的文件路径 容器长ID/name:docker容器路径
# E
## ERROR

## EVO

```yaml
evo_ape tum 0_odom 5_imu_pose_after_trans.txt -va -r full -p --plot_mode xy`

evo_traj kitti KITTI_00_ORB.txt KITTI_00_SPTAM.txt --ref=KITTI_00_gt.txt -p --plot_mode=xz

evo_rpe tum 0_odom 5_imu_pose_after_trans.txt -r full -p --plot_mode xy
```
# F

# G
## GDB
```
gdb -q --args /apollo/bazel-bin/cyber/mainboard/mainboard -d /apollo/modules/omnisense/pose_monitor/dag/inno_pose_monitor.dag

./bazel-bin/modules/omnisense/gps_info_capture/ins_posetrans_test
```
---------
## GIT

```cpp
git init
touch Readme_new
git  add .
git  commit -m '提交的备注信息'
git  push -u origin dev
git remote add origin https://github.com/你的github用户名/你的github仓库.git  
git push origin master
git pull origin master
更新远程分支列表
git remote update origin --prune

查看所有分支
git branch -a

删除远程分支Chapater6
git push origin --delete Chapater6

删除本地分支 Chapater6
git branch -d  Chapater6
    
    //at your own branch
    git add <change>
    git commit -m "***"
    // checkout to destieny branch
    git checkout autonoumous_driving
    git pull
    // back to own branch
    git checkout ad_posemonitor
    git merge autonoumous_driving
    // fix conflict in vscode
    git commit -m "***"
    git push
```
### 从bitbucket下载：

	ssh-add ~/key
	git clone -b dev git@bitbucket.org:ivusw/omnisense.git
	git pull" 来更新您的本地分支
	git reset HEAD <文件>..." 以取消暂存
	git stash  暂存
	git pull   将远程存储库中的更改合并到当前分支中。
----------
# H

# I
## INNOVUSION
### falcon-ros: 

	//改ipv4:10.42.0.91
	http://10.42.0.91:8675/config.pyhtml
	//驱动版本是否匹配？/关网页点云
	//不匹配：
	~/falcon-lidar-sdk/build$ ./download-sdk-release.py ros-melodic-innovusion-driver-release-2.5.0-rc260-public.deb
	sudo dpkg -l | grep inno
	sudo dpkg -r ros-melodic-innovusion-driver-public //卸载旧驱动
	sudo dpkg -i  ros-melodic-innovusion-driver-release-2.5.0-rc260-public.deb
	roslaunch innovusion_pointcloud innovusion_points.launch device_ip:=10.42.0.91
	
	rosbag record /ivpoints   -o /home/xng/data/name.bag
	//bag2pcd
	rosrun pcl_ros bag_to_pcd <input_file.bag> <topic> <output_directory>
# J

# K

# L

# M

# N

# O

# P
## python

```python
        # os.path.dirname (file)返回的是.py文件的目录
        # os.path.abspath (file)返回的是.py文件的绝对路径
        os.path.dirname(os.path.abspath(__file__))
        import os

        print('***获取当前目录***')
        print("当前目录是:{}".format(os.getcwd()))
        print("当前目录是:{}".format(os.path.abspath(os.path.dirname(__file__))))
        print('***获取上级目录***')
        print("上级目录是:{}".format(os.path.abspath(os.path.dirname(os.path.dirname(__file__)))))
        print("上级目录是:{}".format(os.path.abspath(os.path.dirname(os.getcwd()))))
        print("上级目录是:{}".format(os.path.abspath(os.path.join(os.getcwd(), ".."))))
        print('***获取上上级目录***')
        print("上上级目录是:{}".format(os.path.abspath(os.path.join(os.getcwd(), "../.."))))
        //拼接文件路径
        sys.path.append()
```


# Q

# R
## ROS
### fixed frame trans
```
rosrun tf static_transform_publisher 0.0 0.0 0.0 0.0 0.0.0 base_link innovusion 100
```
----------
### Rosbag to pcd/txt:
```
rosrun pcl_ros bag_to_pcd <input_file.bag> <topic> <output_directory>
```

```
rosbag play XXX.bag
rosrun pcl_ros pointcloud_to_pcd input:=/velodyne_points
```

- rosbag topic to txt:

```
rostopic echo -b filename.bag -p /topic > txtname.txt
```
### 查看frame_id

    rostopic echo /topic | grep frame_id
--------------
# S

# T
## torch

- numpy2torch / torch2numpy

```python
tor_datas = torch.from_numpy(data)
tor2numpy=tor_arr.numpy(
```
# U

# V

# W

# X

# Y

# Z

