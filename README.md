# security-handbook · 安全攻防实战手册

[![CI](https://github.com/wdfdsyhb/security-handbook/actions/workflows/lint.yml/badge.svg)](https://github.com/wdfdsyhb/security-handbook/actions/workflows/lint.yml) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> 620 条 bug bounty writeup 和 64+ 份实战资料消化后重排成 22 章中文教程书：顺序读打基础，按目录跳当查询手册，从信息收集一路覆盖到护网红蓝对抗。
>
> English: A 22-chapter Chinese attack-and-defense handbook distilled from 620 bug bounty writeups and 64+ field sources — read it cover to cover or jump in by chapter.

> 整理者：wdfdsyhb · 持续更新 · 最后更新 2026-10-04

## 这本书是什么

把散落在本地各处的学习资料（SRC 技术文档合辑、护网 HW 培训、开源方法论项目、writeup 合集）消化后，按「教程书」逻辑重新组织成 22 章。每章独立成篇，可以顺序读，也可以当查询手册按目录跳。

**内容来源**（四路知识融合）：

| 来源 | 说明 |
|------|------|
| 开源方法论 | PayloadsAllTheThings、OWASP CheatSheet、Awesome-Hacking 等项目深度研读 |
| 国产资料包 | 京东 JSRC 安全小课堂 93 份、绿盟护网培训 19 份、CNVD/SRC 经验分享 |
| writeup 实证 | 620 条真实漏洞披露案例的模式归纳（XSS 290 / RCE 68 / CSRF 49 / SQLi 34 …） |
| 实战检验 | 补天/CNVD/超星等实际报送经验校准 |

## 免责声明

本书所有内容**仅用于授权环境下的安全学习与研究**（自有设备、SRC 平台授权范围、CTF 靶场）。对未授权系统实施任何测试在中国属于违法行为（《网络安全法》《刑法》285/286 条）。书中防御章节可直接用于自检加固，攻击章节请务必遵守授权边界。

## 目录

### 第一部分 · 地图与纪律

- **[01 学习路线与全景图](docs/01-学习路线与全景图.md)** — 行业认知、OWASP Top10 2021、学习路径、本书资料地图
- **[02 方法论总纲](docs/02-方法论总纲.md)** — 五阶段挖掘工作流、证据纪律、定级公式、攻击优先级

### 第二部分 · 侦察

- **[03 信息收集](docs/03-信息收集.md)** — 子域名爆破（破泛解析/递归/占位符）、Github 泄露、字典工程化、IP 段反查、crt.sh 链式打法

### 第三部分 · Web 漏洞逐类击破

- **[04 SQL 注入](docs/04-SQL注入.md)** — 注入分类、盲注方法论、WAF 绕过矩阵、非常规注入点
- **[05 XSS 与 CSRF](docs/05-XSS与CSRF.md)** — DOM XSS 审计、self-XSS 升级三件套、盲 XSS、JSON CSRF
- **[06 文件上传与路径穿越](docs/06-文件上传与路径穿越.md)** — 四层决策树、八条绕过矩阵、解析漏洞
- **[07 SSRF](docs/07-SSRF.md)** — 三大高发入口、IP 混淆/DNS 重绑定、gopher、升级链到 RCE
- **[08 越权与 IDOR](docs/08-越权与IDOR.md)** — A/B 账号对照、空值绕过、签名跨包篡改、流程阶段回验缺失
- **[09 业务逻辑与竞态](docs/09-业务逻辑与竞态.md)** — 密码找回五板斧、交易支付参数矩阵、验证码、HTTP/2 单包攻击
- **[10 认证与会话](docs/10-认证与会话.md)** — JWT 全家桶（kid 注入/密钥混淆）、Header 伪造表、参数污染
- **[11 进阶漏洞](docs/11-进阶漏洞.md)** — 原型污染、HTTP 请求走私、API 安全、GraphQL、XXE/SSTI

### 第四部分 · 服务器与深入

- **[12 服务器与中间件](docs/12-服务器与中间件.md)** — 弱口令、未授权服务 17 清单、Redis 利用、反序列化、四系统基线加固
- **[13 内网渗透](docs/13-内网渗透.md)** — 域环境、横向移动、信息收集两分法、留痕规避
- **[14 WAF 对抗](docs/14-WAF对抗.md)** — 五类绕过框架、检测范围填充、用例化绕过集

### 第五部分 · 移动与硬件

- **[15 移动端安全](docs/15-移动端安全.md)** — APP 加固与脱壳、证书锁定绕过、小程序攻击面
- **[16 IoT 与硬件安全](docs/16-IoT与硬件安全.md)** — 固件提取、UART/JTAG、无线协议、故障注入

### 第六部分 · 护网 HW（攻防两张图）

- **[17 护网红队篇](docs/17-护网HW-红队篇.md)** — 计分规则、12 类攻击手法链、降噪与取证规避
- **[18 护网蓝队篇](docs/18-护网HW-蓝队篇.md)** — 检测三角、安全设备产品线、应急三要素法、告警判定公式

### 第七部分 · 产出与合规

- **[19 SRC 实战与 CNVD 报送](docs/19-SRC实战与CNVD.md)** — 平台选择、CNVD 证书细则、报告五要素、危害提升链
- **[20 法规与合规红线](docs/20-法规与合规红线.md)** — 《网络产品安全漏洞管理规定》红线、相关法律要点
- **[21 Kali 与工具箱](docs/21-Kali与工具箱.md)** — 全工具索引、Kali VM 环境、手机端方案
- **[22 面试题库速查](docs/22-面试题库速查.md)** — 护网/安全岗高频面试题与答案要点

### 附录

- **[writeup 模式库](writeups/writeup模式库.md)** — 620 条 writeup 的分类分布、高频漏洞发现模式、20 篇最值得精读清单

## 怎么用这本书

1. **系统学**：按章节顺序读，01→02→03 打基础，第三部分逐类击破。
2. **实战查**：拿到目标先看 03（信息收集），命中哪类洞就翻对应章节的 checklist。
3. **面试备**：22 章速查 + 17/18 章攻防两张图。
4. **写报告**：19 章模板 + 20 章合规自检。

---

## English

Full English documentation: [README_EN.md](README_EN.md)
