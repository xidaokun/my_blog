---
title: Car OTA
date: 2023-04-16 13:37:58
tags: Architecture Car
---


### 简介
汽车远程升级技术OTA(Over-The-Air)，包括FOTA和SOTA。
FOTA（Firmware-Over-The-Air），固件在线升级，升级影响大，会影响整个系统；
SOTA（Software-Over-Thre-Air），软件在线升级，只影响部分应用app。


### 功能
1，架构
{% asset_img ota_link.png ota link %}


2，云端OTA服务
- 运维人员通过web运行平台管理OTA；
- 资源管理包括：全量升级资源，差分升级资源，补丁升级资源；
- 车型管理：不同车型固件和软件往往有很大区别，所以要针对不同的车型指定不同的发布策略；
- 升级管理：包括全量升级，补丁升级，差分升级；可以配置是否静默升级；可以配置升级时间等信息；
- 发布策略：可以指定车辆或车型发布，也可以灰度测试发布。
{% asset_img ota_cloud.png ota cloud link %}


3，车端OTA终端
- 终端通过安全链路通信，接收升级通知或主动检查升级；
- 有更新时，终端同步升级信息，并下载升级包，同时存储这些信息；
- 检查升级策略，调度子系统分发升级；
- 另外，为了兼容不同的软件和设备，不同的子系统需要根据标准做是配。
{% asset_img ota_car.png ota car link %}










