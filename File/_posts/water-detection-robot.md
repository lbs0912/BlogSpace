---
title: 水域侦查机器人
date: 2017-03-10 15:35:26
categories: Technology
tags: [Robot,Technology]
keywords: Robot, Technology
toc: true
---

* 为了以较低成本实现水域信息采集，本文设计了一种采用圆形船体的水域侦察机器人，该水域侦察机器人通过传感器、摄像头和无线模块采集并回传图像和水域信息。
* 跳转到[我的Github](https://github.com/lbs0912/Waters-Detection-Robots)阅读。
<!--more-->

## 概述
为了以较低成本实现水域信息采集，本文设计了一种采用圆形船体的水域侦察机器人，该水域侦察机器人通过传感器、摄像头和无线模块采集并回传图像和水域信息。机器人主要依靠水流推动前进，利用四个微型水泵作为驱动装置摆脱死水区域。对水域侦察机器人进水域信息采集实验，结果表明该水域侦察机器人以较低的成本实现了水域信息采集功能。


![机器人展示2](http://ol3kbaay9.bkt.clouddn.com/robot-1.jpg)




水域侦察机器人的总体设计框图如下图所示，包括主控核心单元、水泵驱动单元、水域信息采集单元、能量供应单元以及无线通信单元。

![水域侦察机器人的总体设计框图](http://ol3kbaay9.bkt.clouddn.com/whole-robot-fig.jpg)

## 硬件设计
机器人的微控制器采用意法半导体公司生产的STM32F103VET6芯片。水域侦察机器人的GPS模块、JENNIC 无线模块、串口摄像头和程序调试各占用一个UART接口，在STM32F103VET6芯片的协调控制下，水域侦察机器人可以稳定工作，实现基本的设计功能。电路功能模块设计如下图所示。

![电路功能模块设计](http://ol3kbaay9.bkt.clouddn.com/%E7%94%B5%E8%B7%AF%E5%8A%9F%E8%83%BD%E6%A8%A1%E5%9D%97%E8%AE%BE%E8%AE%A1.jpg)



系统中，水泵用于控制船体的运动。水泵对船体的运动控制如下图所示。
![船体运动控制示意图](http://ol3kbaay9.bkt.clouddn.com/%E8%88%B9%E4%BD%93%E8%BF%90%E5%8A%A8%E6%8E%A7%E5%88%B6%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

## 软件设计
系统中采用VC6.0编写上位机控制界面，控制界面如下图所示。
![系统工作界面](http://ol3kbaay9.bkt.clouddn.com/%E7%B3%BB%E7%BB%9F%E6%8E%A7%E5%88%B6%E7%95%8C%E9%9D%A2.jpg)

软件工作流程图如下图所示。
![软件工作流程图](http://ol3kbaay9.bkt.clouddn.com/%E8%BD%AF%E4%BB%B6%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B%E5%9B%BE.jpg)

## 实验测试
* 水中测试实验
![水中测试实验](http://ol3kbaay9.bkt.clouddn.com/%E6%B0%B4%E4%B8%AD%E6%B5%8B%E8%AF%95%E5%AE%9E%E9%AA%8C.jpg)
* 系统测试视频：[点击播放](https://v.youku.com/v_show/id_XMjYxNTA5NTY0MA==.html?spm=a2h0w.8278793.2736843.4)
* 系统定位精度对比图
![定位精度对比图](http://ol3kbaay9.bkt.clouddn.com/%E5%AE%9A%E4%BD%8D%E7%B2%BE%E5%BA%A6%E5%AF%B9%E6%AF%94%E5%9B%BE.jpg)

## 项目成果
* 系统控制软件:[点击获取](https://github.com/lbs0912/Waters-Detection-Robots)
* 发表[中文期刊](http://ol3kbaay9.bkt.clouddn.com/%E3%80%90%E7%BB%88%E7%A8%BF%E6%A8%A1%E6%9D%BF%E3%80%91%E6%B0%B4%E5%9F%9F%E4%BE%A6%E5%AF%9F%E6%9C%BA%E5%99%A8%E4%BA%BAV2.0-%E5%BC%A0%E7%91%9E2014-12-3.pdf)1篇
* 申请[国家发明专利](http://ol3kbaay9.bkt.clouddn.com/%E3%80%90%E4%B8%93%E5%88%A9%E5%8F%97%E7%90%86%E9%80%9A%E7%9F%A5%E3%80%91%E4%B8%80%E7%A7%8D%E7%94%A8%E4%BA%8E%E6%B0%B4%E5%9F%9F%E4%BF%A1%E6%81%AF%E9%87%87%E9%9B%86%E7%9A%84%E6%B0%B4%E5%9F%9F%E4%BE%A6%E5%AF%9F%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%8F%8A%E5%85%B6%E6%8E%A7%E5%88%B6%E6%96%B9%E6%B3%952014-12-4.pdf)1项
* [项目成果展板](http://ol3kbaay9.bkt.clouddn.com/%E9%A1%B9%E7%9B%AE%E6%88%90%E6%9E%9C%E5%B1%95%E6%9D%BF.jpg)
* [项目演示文稿](http://ol3kbaay9.bkt.clouddn.com/%E6%B0%B4%E5%9F%9F%E4%BE%A6%E5%AF%9F%E6%9C%BA%E5%99%A8%E4%BA%BA_%E9%A1%B9%E7%9B%AE%E7%BB%93%E9%A2%98%E7%AD%94%E8%BE%A9_new_2014-12-03.pptx)
* [系统主板电路图](http://ol3kbaay9.bkt.clouddn.com/%E4%B8%BB%E6%8E%A7%E6%9D%BFRobot%201.2.pdf)
* [系统电路设计图](http://ol3kbaay9.bkt.clouddn.com/%E4%B8%BB%E6%8E%A7%E6%9D%BFrobot4.0.pdf)
* [结题报告](http://ol3kbaay9.bkt.clouddn.com/%E6%B0%B4%E5%9F%9F%E4%BE%A6%E5%AF%9F%E6%9C%BA%E5%99%A8%E4%BA%BA%E2%80%94%E2%80%94%E7%BB%93%E9%A2%98%E6%8A%A5%E5%91%8A1.4%202014-12-4.pdf)


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
