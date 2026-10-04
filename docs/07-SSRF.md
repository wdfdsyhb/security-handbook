# 07 SSRF

## 三大高发入口（writeup 实证）

1. **PDF 生成器**（html2pdf、wkhtmltopdf——报告/发票功能）
2. **媒体处理**：ffmpeg、ImageMagick、phantomjs（图片地址最常见）
3. **URL 预览 / webhook**（分享卡片、回调地址）

本质：用户可控 URL + 未对目标地址过滤限制。

## 三类攻击

- **内网指纹**：banner、端口、中间件版本探测
- **攻击内网应用**：弱口令、struts2 命令执行、Discuz SSRF + redis getshell
- **脆弱组件+协议**：`ftp://`、`file://`、`gopher://`、`dict://`；FFmpeg 任意文件读取、XXE

## 绕过矩阵

### IP 混淆

| 写法 | 等价 |
|------|------|
| `http://2130706433/` | `127.0.0.1`（十进制） |
| `http://2852039166/` | `169.254.169.254`（AWS 元数据） |
| `http://0177.0.0.1/` | 八进制 |
| `http://0x7f000001` | 十六进制 |
| `http://[::]:80/` | IPv6 回环 |
| `http://127.1` / `http://0/` | 简写 |

### DNS 重绑定

域名在攻击者 IP 和目标 IP 之间交替解析，绕过只在验证时解析一次 DNS 的过滤器。工具：`1u.ms`、`r3dir.me`。

### 302/307 跳转

白名单域名 302 跳转到内网目标；**307/308 保留 HTTP 方法和 Body**（对 POST SSRF 关键）。`https://307.r3dir.me/--to/?url=http://localhost`。

### URL 解析差异

```
http://127.1.1.1:80\@127.2.2.2:80/
http://127.1.1.1:80#\@127.2.2.2:80/
```
urllib2、requests、浏览器解析结果不同——解析器混淆。

### 协议走私

| 协议 | 用途 |
|------|------|
| `file://` | 读本地文件 |
| `gopher://` | 发原始 TCP payload（打内网 Redis/MySQL/SMTP） |
| `dict://` | DICT 服务交互 |
| `ldap://` / `netdoc://` | 目录查询 / Java 文件读取 |

## 云元数据端点（最高价值）

```
AWS:    http://169.254.169.254/latest/meta-data/
GCP:    http://metadata.google.internal/computeMetadata/v1/
阿里云:  http://100.100.100.200/latest/meta-data/
```

SSRF 打到云元数据 = 拿临时凭证 = 最高赏金潜力。

## 升级链

- SSRF → 文件读 → 云凭证 → RCE
- SSRF → 内网 Redis 未授权（gopher 写 crontab/公钥）→ RCE
- SSRF → 内网 MySQL 未授权 → RCE
- LFI → session/日志 poison → RCE（姊妹篇）
- WordPress pingback SSRF：经 gethostbyname 发起，检测看请求域名

## 盲 SSRF 与检测

- 盲 SSRF 用 **Interactsh**（自建 OOB 收集器）证明
- 检测特征：接口异常请求内网 IP、日志连续、时间段请求量明显

## 防御判定清单

过滤返回信息、统一错误信息、限端口 80/443/8080/8090、黑名单内网 IP、仅允许 http/https、**正则校验防畸形绕过**（解析器差异是根因，最好用 URL 解析库做目标归一化后再判断）。

## 工具

| 工具 | 用途 |
|------|------|
| SSRFmap | 自动 fuzz + 利用 |
| Gopherus | 生成 gopher payload |
| See-SURF | SSRF 参数扫描 |
| Interactsh | OOB 交互收集 |
