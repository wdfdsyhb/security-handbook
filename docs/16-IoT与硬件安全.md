# 16 IoT 与硬件安全

> 工控/车联网/IoT 是蓝海：赏金高、竞争少，CNNVD 对 IoT 漏洞报送友好（自购设备→固件提取→漏洞→报送是可行路线）。**默认凭证仍是 IoT 第一攻击向量**（Mirai 僵尸网络证明）。

## 完整渗透流程

### 1. 侦察

- 识别设备型号、固件版本、暴露服务
- 默认凭证数据库（Router Passwords）；搜索已知 CVE
- 智能设备/智能硬件挖掘入门：先看可联网面（app/API/云），再看物理面（UART/JTAG）

### 2. 固件获取

- **官网下载**（最容易，多数厂商不加密）
- UART/JTAG 提取（物理访问）
- 在线更新包截获（中间人）
- 工具：**Binwalk**、FACT、EMBA、Firmadyne

### 3. 静态分析

```bash
binwalk -e firmware.bin          # 提取文件系统
firmwalker firmware/             # 搜索硬编码凭证
# Ghidra/IDA Pro/Radare2 分析二进制
```

找什么：硬编码密码/密钥、shadow 文件、启动脚本里的 curl/wget 后门、私有协议解析代码、证书。

### 4. 动态分析

- **Firmadyne / FirmAE**：仿真固件跑起来再测（web 界面直接测注入/未授权）
- **Boofuzz / AFL++**：模糊测试（协议 fuzz）
- Saleae：逻辑分析仪监控通信

### 5. 硬件攻击

- **UART**：识别 PCB 焊盘（TX/RX/GND）→ 连 USB-TTL → root shell → 提取固件。**UART 调试口经常无保护 = 物理访问 = root shell**
- **JTAG/SWD**：OpenOCD、JTAGenum、SEGGER J-Link；路径：JTAG → dump 固件 → 离线分析 → 找漏洞
- 工具：Bus Pirate、Shikra、Glasgow、GreatFET

### 6. 无线协议

| 协议 | 工具 |
|------|------|
| BLE | Ubertooth One、Btlejack、Sniffle |
| ZigBee | KillerBee、ApiMote |
| SDR | RTL-SDR、HackRF One、BladeRF |
| RFID/NFC | Proxmark 3 RDV4、ChameleonUltra |
| WiFi | bettercap、aircrack-ng（WPA 抓握手包+字典） |

### 7. 故障注入（高级）

- 电压毛刺：ChipWhisperer、Pico Glitcher
- 电磁故障注入：ChipSHOUTER
- 目标：绕过安全启动（STM32/ESP32 闪存加密绕过已有公开案例）

## 真实案例参考

| 目标 | 漏洞 | 影响 |
|------|------|------|
| D-Link DWR-932B | 多漏洞 | 完全控制 |
| Huawei HG533 | UART 固件逆向 | 凭证提取 |
| Philips Hue Bridge | root 访问 | 完全控制 |
| SpaceX Starlink | 毛刺攻击 | 用户终端沦陷 |
| ESP32-C3/C6 | 故障注入 | 闪存加密绕过 |

## 车联网/智能设备（JSRC 视角补充）

- 汽车安全面：OBD 接口、CAN 总线、T-Box 远程、车机 Android 系统、蓝牙钥匙
- 智能设备挖掘：app 层（API 越权/未授权）→ 云层（厂商云 API）→ 设备层（固件/端口）
- NFC 支付安全：中继攻击、卡片克隆（Proxmark）

## 报送路线（CNNVD/CNVD）

自购设备 → 固件提取 → 漏洞发现 → 按第 19 章报送流程产出编号/证书。白盒案例只交代码调试过程，不混黑盒案例。
