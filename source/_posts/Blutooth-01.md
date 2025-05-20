---
title: Blutooth-01
date: 2025-05-14 14:10:28
tags: Blutooth
---

### 概述
本章为蓝牙协议简介篇。


### 蓝牙协议
蓝牙协议包括经典蓝牙（Classic Bluetooth）、低功耗蓝牙（BLE, Bluetooth Low Energy）和通用协议。

### 经典蓝牙协议（BR/EDR + AMP）
 **1. 控制器层（Controller Layer）**
物理层（PHY）：工作在2.4GHz ISM频段，使用跳频扩频（FHSS）技术。

链路控制层（Baseband Layer）：管理物理链路，处理连接建立、数据包格式和加密。

链路管理协议（LMP, Link Manager Protocol）：管理设备配对、功率控制、角色切换等。

 **2. 主机层（Host Layer）**
主机控制器接口（HCI, Host Controller Interface）：连接蓝牙硬件（控制器）和软件（主机）的通信接口。

逻辑链路控制与适配协议（L2CAP, Logical Link Control and Adaptation Protocol）：提供多路复用、数据分段和重组功能。

服务发现协议（SDP, Service Discovery Protocol）：用于发现其他蓝牙设备提供的服务。

RFCOMM（串口仿真协议）：模拟RS-232串口，支持传统串口应用（如蓝牙打印机）。

OBEX（对象交换协议）：用于文件传输（FTP）和联系人同步。

音频协议（如SCO/eSCO）：支持语音通话（如蓝牙耳机）。

 **3. 应用层（Profile Layer）**
A2DP（高级音频分发协议）：高质量音频传输（如音乐播放）。

HFP（免提协议）：语音通话控制。

HID（人机接口设备协议）：支持键盘、鼠标等外设。

AVRCP（音视频远程控制协议）：设备远程控制。

### 低功耗蓝牙协议（BLE）
BLE专为低功耗、间歇性数据传输设计（如物联网设备）。

 **1. 控制器层（Controller Layer）**
物理层（PHY）：同样使用2.4GHz频段，但信道更少（40个）。

链路层（Link Layer）：管理广播、扫描、连接建立和数据传输。

 **2. 主机层（Host Layer）**
主机控制器接口（HCI）：与经典蓝牙类似，但针对BLE优化。

逻辑链路控制与适配协议（L2CAP）：简化版，支持更小的数据包。

属性协议（ATT, Attribute Protocol）：定义设备间数据交换的格式（如传感器数据）。

通用属性规范（GATT, Generic Attribute Profile）：基于ATT，定义服务（Service）、特征（Characteristic）等数据结构。

安全管理协议（SM, Security Manager）：处理配对和加密。

 **3. 应用层**
通用访问规范（GAP, Generic Access Profile）：管理设备发现、连接和广播。

自定义Profile：开发者可基于GATT定义特定服务（如心率监测、温度传感器）。

### 通用协议
安全管理（Security）：包括配对、身份验证和数据加密（如AES-CCM）。
测试规范（Qualification）：蓝牙技术联盟（SIG）制定的认证标准（如QDID）。












