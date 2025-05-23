---
title: Blutooth-05
date: 2025-05-20 14:50:30
tags:
---

### 概述
本章为经典蓝牙和Ble差异。

### **1. 设计目标与适用场景**
| **维度**       | **经典蓝牙 (BR/EDR)**                | **低功耗蓝牙 (BLE)**                 |
|-----------------|--------------------------------------|--------------------------------------|
| **功耗**       | 高功耗（持续连接，适合大流量传输）   | 极低功耗（间歇性通信，适合电池供电设备） |
| **数据速率**   | 高（1-3 Mbps，适合音频、文件传输）   | 低（1 Mbps以下，适合小数据包传输）     |
| **典型应用**   | 耳机、音箱、文件传输                 | 传感器、健康设备、IoT设备             |

---

### **2. 设备发现与连接流程**
#### **经典蓝牙 (BR/EDR)**
1. **设备发现**  
   - 使用 `BluetoothAdapter.startDiscovery()` 扫描周围设备。
   - 设备通过 **可发现模式**（Discoverable Mode）广播自身存在。
   - 发现结果通过 `BroadcastReceiver` 返回设备列表（包含名称、MAC地址）。

2. **配对与连接**  
   - **配对**：需用户手动确认（PIN码或弹窗），建立安全链路。
   - **连接**：通过 `BluetoothSocket` 建立RFCOMM或L2CAP通道（类似TCP）。
   - **数据传输**：基于流式Socket通信（持续连接）。

#### **BLE**
1. **设备发现**  
   - 通过 **广播包**（Advertising Packet）主动发送设备信息（无需配对）。
   - 扫描端使用 `BluetoothLeScanner.startScan()` 过滤目标设备（基于MAC地址、服务UUID等）。

2. **连接与通信**  
   - **连接**：直接调用 `connectGatt()` 建立连接，无需配对（但可加密）。
   - **服务发现**：连接后自动触发服务发现（GATT服务树）。
   - **数据传输**：基于 **特征值（Characteristics）** 的读写、通知（Notification）或指示（Indication）。

---

### **3. 数据传输模式**
| **维度**       | **经典蓝牙**                          | **BLE**                              |
|-----------------|--------------------------------------|--------------------------------------|
| **通信模型**   | 流式传输（持续连接，双向通道）        | 短数据包（按需通信，事件驱动）        |
| **协议栈**     | RFCOMM（串口模拟）、L2CAP、A2DP等    | GATT（Generic Attribute Profile）    |
| **数据交互**   | 基于Socket的InputStream/OutputStream | 读写特征值、启用通知/指示            |

---

### **4. 权限与API差异**
#### **Android端代码示例**
- **经典蓝牙**  
  ```java
  // 设备发现
  BluetoothAdapter adapter = BluetoothAdapter.getDefaultAdapter();
  adapter.startDiscovery();

  // 连接与通信（RFCOMM）
  BluetoothDevice device = adapter.getRemoteDevice(macAddress);
  BluetoothSocket socket = device.createRfcommSocketToServiceRecord(UUID.randomUUID());
  socket.connect();
  OutputStream out = socket.getOutputStream();
  out.write(data);
  ```

- **BLE**  
  ```java
  // 扫描设备
  BluetoothLeScanner scanner = adapter.getBluetoothLeScanner();
  scanner.startScan(scanCallback);

  // 连接与通信（GATT）
  BluetoothGatt gatt = device.connectGatt(context, false, gattCallback);
  gatt.discoverServices();
  BluetoothGattCharacteristic characteristic = service.getCharacteristic(uuid);
  gatt.writeCharacteristic(characteristic);
  ```

---

### **5. 安全与加密**
| **维度**       | **经典蓝牙**                          | **BLE**                              |
|-----------------|--------------------------------------|--------------------------------------|
| **配对**       | 强制配对（PIN码或弹窗）               | 可选加密（通过LE Secure Connections） |
| **加密时机**   | 连接前配对阶段                        | 连接后按需加密（如读写敏感特征值）    |

---

### **6. 功耗管理**
- **经典蓝牙**  
  - 持续连接，适合实时数据传输（如音频流），但功耗高。
  - 需手动断开连接释放资源。

- **BLE**  
  - **连接参数优化**：通过调整连接间隔（Connection Interval）和从机延迟（Slave Latency）降低功耗。
  - **广播优化**：设备可配置为仅在特定时间广播（减少空闲功耗）。

---

### **7. 典型问题与调试**
| **问题类型**     | **经典蓝牙**                          | **BLE**                              |
|------------------|--------------------------------------|--------------------------------------|
| **连接失败**    | 配对冲突、协议不兼容                 | 服务未发现、特征权限配置错误         |
| **数据传输卡顿**| 带宽不足、信号干扰                   | 连接间隔过长、MTU大小限制            |
| **兼容性问题**  | 不同厂商的RFCOMM实现差异             | GATT服务结构不符合规范               |

---

### **总结**
- **选择依据**：  
  - 需要高速、持续传输（如音频） → **经典蓝牙**。  
  - 需要低功耗、间歇性通信（如传感器） → **BLE**。  
- **开发注意事项**：  
  - 经典蓝牙需处理配对流程，BLE需关注GATT服务结构。  
  - BLE对Android版本兼容性更敏感（如Android 4.3+支持BLE）。