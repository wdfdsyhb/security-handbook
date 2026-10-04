# 21 Kali 与工具箱

> 工具是能力的复制，不是能力本身；能写插件才是分水岭。工具谱系入口：sectools.org。

## 全工具索引（按工作流阶段）

### 信息收集
| 工具 | 用途 |
|------|------|
| subfinder / amass / OneForAll | 子域名发现 |
| subDomainsBrute / layer | 爆破型子域名枚举 |
| nmap / RustScan / masscan | 端口扫描（masscan 注意 WAF 假端口） |
| dnsenum | DNS 枚举+区域传送 |
| crt.sh / Censys | 证书透明度 |
| theharvester | 邮箱/子域社工收集 |
| sherlock / maigret | 用户名跨平台枚举 |
| bbot | 自动化侦察流水线 |
| FOFA / Shodan / 鹰图 | 空间测绘（国内用 FOFA/鹰图） |

### Web 渗透
| 工具 | 用途 |
|------|------|
| **Burp Suite** | 综合平台（插件：Authz/Autorize/AuthMatrix、Turbo Intruder、HTTP Request Smuggler、JWT Editor、burp-suite-error-message-checks、Header Analyzer） |
| sqlmap | SQL 注入自动化（--tamper 绕 WAF） |
| XSStrike / Dalfox | XSS 检测 |
| ffuf / dirsearch / BBscan | 目录与参数 fuzz |
| Nuclei | 模板化漏扫（CVE POC 库） |
| mitmproxy | TLS 拦截代理 |
| xray | 被动扫描（配合 Burp） |

### 越权/认证
| 工具 | 用途 |
|------|------|
| jwt_tool | JWT 攻击瑞士军刀（-X a none / -X k 混淆 / -X i 注入 / -C 爆破） |
| hashcat | GPU 爆破（-m 16500 JWT） |
| hydra | 在线爆破（SSH/FTP/表单） |
| John the ripper | 离线哈希破解 |

### SSRF/走私/竞态
| 工具 | 用途 |
|------|------|
| SSRFmap / Gopherus | SSRF 利用 |
| Interactsh | OOB 外带收集（盲打证明） |
| Smuggler | 请求走私 |
| Turbo Intruder / h2spacex | 竞态 |

### 移动端
| 工具 | 用途 |
|------|------|
| jadx / apktool / MobSF | APK 反编译与静态分析 |
| Frida / objection | 动态 hook 与证书解锁 |
| Drozer | Android 组件测试 |

### IoT/硬件
| 工具 | 用途 |
|------|------|
| Binwalk / firmwalker | 固件提取与敏感信息搜索 |
| FACT / EMBA / Firmadyne / FirmAE | 固件分析与仿真 |
| OpenOCD | JTAG 调试 |
| bettercap | 无线/中间人 |
| aircrack-ng | WiFi（WPA 抓握手包+字典） |
| Proxmark 3 | RFID/NFC |

### 辅助
| 工具 | 用途 |
|------|------|
| CyberChef | 编解码/数据转换 |
| SecLists / fuzzdb | 字典库 |
| GitMiner / gitrob / weakfilescan | Github 泄露 |
| Gitleaks | 密钥扫描（代码库自检） |
| Trivy | 容器/依赖漏扫 |
| Postman / jq | API 测试 |

## Kali VM 实战环境（本机已部署参考架构）

- **Kali VM**（VMware NAT）：安全工具主战场；OSINT 工具集已装 sherlock/maigret/bbot/strix/vulscan
- **代理穿透**：VM 走宿主代理 `192.168.98.1:7897`（Clash Verge）——访问 Github/海外资源必需；代理配置失效先查宿主残留系统代理
- **自动化平台**：HexStrike（MCP 化安全工具编排，端口 8888）——AI agent 驱动的渗透工具调度
- **远程自动化**：paramiko SSH 脚本批量下发命令（密码/口令爆破脚本自写优于工具堆叠）
- Windows 宿主侧辅助：Gitleaks（PATH 已配）、Trivy、Caddy（内网靶场）

## 手机端方案（NetHunter，玩具属性）

Termux → `termux-change-repo` 换 USTC 源 → 装 NetHunter 脚本 → `nh kex` 开 VNC(5901) 图形界面。无 root 可用，但脚本依赖外网且版本老旧，**真干活用 Kali VM**。

WiFi 审计最小流程（aircrack-ng）：
```bash
airmon-ng start wlan0          # 监控模式
airodump-ng --channel x --bssid xx --write cap wlan0mon
aireplay-ng -0 0 -a AP wlan0mon   # deauth 抓握手包（教程常漏这步）
aircrack-ng -w 字典 xxx.cap       # WPA 字典破解
```

## 字典资源

- SecLists（Daniel Miessler）——全类型
- 自建：1000+ 开源 web 源码正则提取目录/脚本/参数/js 文件名四类字典，按命中次数排序（见 03 章）
- rapid7 fdns/rnds 开放数据——子域名
- 弱口令：默认口令表（网络设备/管理卡）+ 行业口令 + 人名拼音/手机号组合
