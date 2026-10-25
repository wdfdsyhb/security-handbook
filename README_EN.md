# security-handbook · Security Attack & Defense Handbook

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> 620 bug bounty writeups and 64+ field sources, digested and reorganized into a 22-chapter Chinese handbook: read it cover to cover, or jump in by chapter as a lookup manual, from reconnaissance all the way to red-vs-blue exercise playbooks.
>
> English: A 22-chapter Chinese attack-and-defense handbook distilled from 620 bug bounty writeups and 64+ field sources — read it cover to cover or jump in by chapter.

> Compiled by wdfdsyhb · continuously updated · last updated 2026-10-04

## What this book is

Learning material scattered across local storage (SRC technical document collections, HW (HVV-style) attack-defense training decks, open-source methodology projects, writeup collections) has been digested and reorganized, following textbook logic, into 22 chapters. Each chapter stands alone: read them in order, or use the table of contents to jump straight to what you need.

**Sources** (four streams of knowledge merged):

| Source | Description |
|------|------|
| Open-source methodology | Deep reading of PayloadsAllTheThings, OWASP CheatSheet, Awesome-Hacking and similar projects |
| Chinese domestic packs | 93 JD JSRC security lecture decks, 19 NSFOCUS HW training decks, CNVD/SRC experience sharing |
| Writeup evidence | Pattern analysis of 620 real vulnerability disclosure cases (XSS 290 / RCE 68 / CSRF 49 / SQLi 34 …) |
| Field validation | Calibrated against actual submissions on Butian/CNVD/Chaoxing platforms |

## Disclaimer

Everything in this book is **for security learning and research in authorized environments only** (your own devices, authorized SRC scopes, CTF labs). Testing unauthorized systems is illegal in China (Cybersecurity Law; Criminal Law articles 285/286). The defense chapters can be used directly for self-assessment and hardening; when using the attack chapters, respect your authorization boundaries.

## Table of Contents

### Part 1 · Map and Discipline

- **[01 Learning Roadmap & Panorama](docs/01-学习路线与全景图.md)** — Industry overview, OWASP Top 10 2021, learning paths, the book's source map
- **[02 Methodology Overview](docs/02-方法论总纲.md)** — Five-stage hunting workflow, evidence discipline, severity formulas, attack prioritization

### Part 2 · Reconnaissance

- **[03 Information Gathering](docs/03-信息收集.md)** — Subdomain bruteforce (beating wildcard DNS / recursion / placeholders), GitHub leaks, engineered wordlists, reverse IP lookup, chained crt.sh playbooks

### Part 3 · Web Vulnerabilities, One Class at a Time

- **[04 SQL Injection](docs/04-SQL注入.md)** — Injection taxonomy, blind-injection methodology, WAF bypass matrix, unconventional injection points
- **[05 XSS and CSRF](docs/05-XSS与CSRF.md)** — DOM XSS auditing, the three-step self-XSS escalation, blind XSS, JSON CSRF
- **[06 File Upload and Path Traversal](docs/06-文件上传与路径穿越.md)** — Four-layer decision tree, eight bypass vectors, parser vulnerabilities
- **[07 SSRF](docs/07-SSRF.md)** — Three high-frequency entry points, IP obfuscation/DNS rebinding, gopher, escalation chains to RCE
- **[08 Broken Access Control and IDOR](docs/08-越权与IDOR.md)** — A/B account comparison, empty-value bypasses, cross-request signature tampering, missing per-stage re-validation
- **[09 Business Logic and Race Conditions](docs/09-业务逻辑与竞态.md)** — Five plays for password reset, payment parameter matrix, captchas, HTTP/2 single-packet attacks
- **[10 Authentication and Sessions](docs/10-认证与会话.md)** — The JWT family (kid injection / key confusion), header spoofing tables, parameter pollution
- **[11 Advanced Vulnerabilities](docs/11-进阶漏洞.md)** — Prototype pollution, HTTP request smuggling, API security, GraphQL, XXE/SSTI

### Part 4 · Servers and Beyond

- **[12 Servers and Middleware](docs/12-服务器与中间件.md)** — Weak credentials, 17 unauthorized-service checks, Redis exploitation, deserialization, baseline hardening for four OSes
- **[13 Internal Network Penetration](docs/13-内网渗透.md)** — Domain environments, lateral movement, two-way recon methods, trace avoidance
- **[14 WAF Evasion](docs/14-WAF对抗.md)** — Five bypass frameworks, detection-scope stuffing, case-based bypass collections

### Part 5 · Mobile and Hardware

- **[15 Mobile Security](docs/15-移动端安全.md)** — App hardening and unpacking, certificate pinning bypass, mini-program attack surface
- **[16 IoT and Hardware Security](docs/16-IoT与硬件安全.md)** — Firmware extraction, UART/JTAG, wireless protocols, fault injection

### Part 6 · HW Attack-Defense Exercises (two maps, red and blue)

- **[17 HW Red Team](docs/17-护网HW-红队篇.md)** — Scoring rules, 12 attack technique chains, noise reduction and forensic evasion
- **[18 HW Blue Team](docs/18-护网HW-蓝队篇.md)** — The detection triangle, security product lines, three-element incident response, alert triage formulas

### Part 7 · Output and Compliance

- **[19 SRC in Practice and CNVD Submissions](docs/19-SRC实战与CNVD.md)** — Platform selection, CNVD certificate details, five elements of a report, severity escalation chains
- **[20 Regulations and Compliance Red Lines](docs/20-法规与合规红线.md)** — Red lines of the Provisions on the Management of Network Product Security Vulnerabilities, key points of related laws
- **[21 Kali and the Toolbox](docs/21-Kali与工具箱.md)** — Full tool index, Kali VM setup, mobile options
- **[22 Interview Question Bank](docs/22-面试题库速查.md)** — High-frequency HW/security-role interview questions with answer points

### Appendix

- **[Writeup Pattern Library](writeups/writeup模式库.md)** — Category distribution of the 620 writeups, high-frequency discovery patterns, 20 most worth reading closely

## How to use this book

1. **Systematic learning**: read in chapter order; 01→02→03 lay the foundation, then work through Part 3 one vulnerability class at a time.
2. **Field lookup**: start with 03 (recon) on any target; when you hit a vulnerability class, open that chapter's checklist.
3. **Interview prep**: the 22-chapter quick reference + the red/blue maps in 17/18.
4. **Report writing**: templates from 19 + compliance self-checks from 20.

> 🇨🇳 中文文档: [README.md](README.md)
