# Bug_Bounty_writeups 合集研读（620 条）

> 来源：alexbieber/Bug_Bounty_writeups（已克隆 D:\zzzz\tools\Bug_Bounty_writeups\）
> 研读日期：2026-09-28

## 分类分布

| 类别 | 数量级 | 备注 |
|---|---|---|
| XSS | ~290 | 占近半，含大量 self-XSS 升级链 |
| RCE | ~68 | 链式利用最多 |
| CSRF | ~49 | 一半以上通向 ATO |
| SQLi | ~34 | 非常规注入点占比高 |
| SSRF | ~27 | 几乎全是升级链 |
| Subdomain Takeover | ~22 | — |
| Clickjacking / CORS / LFI / Auth Bypass / Race / DoS / 2FA | 各 10-21 | — |
| IDOR | 仅 5（全 Facebook） | 覆盖不足 |
| Buffer Overflow / Android | 教程非 writeup | 数据质量注水 |

## 反复出现的漏洞发现模式

### XSS
1. **Self-XSS 升级三件套**：① self-XSS + 登录页 CSRF → stored XSS；② self-XSS + Clickjacking 诱导粘贴；③ 借授权缺陷写入他人会话。"self XSS to ..." 出现 15+ 次，最高频套路
2. **Blind XSS 打后台**：客服工单、联系表单、简历文件名、User-Agent、报错回执邮件——payload 等管理员打开后台才触发
3. **文件与 Content-Type 入口**：SVG/PNG content-type、MIME sniffing、polyglot 图片、文件名、markdown 编辑器、Open Graph embed
4. **WAF/CSP 绕过靠换语法**：AngularJS 模板注入、大小写/Unicode/编码、CSP 白名单域上的可利用端点
5. **影响升级决定等级**：XSS→窃取凭据→ATO；反射型靠 HTTP smuggling/缓存投毒变存储型

### CSRF
1. 价值标准 = 危害链：改邮箱/密码 CSRF → ATO
2. JSON CSRF + Content-Type/Method 覆盖绕过校验
3. Site-wide CSRF：一处不校验 → 全站无防护推断
4. Token 缺陷四件套：删 token、token 不绑会话、Referer 伪造、GET/POST 双提交

### Clickjacking
1. 单独提报低危；有效场景 = 一步敏感操作无确认（Google myaccount 7500 刀）
2. 组合公式：+self-XSS、绕 CSRF、HTML5 drag&drop 泄 token
3. 高发：Web 端即时通讯（Telegram/WhatsApp Web）

### LFI / SSRF
1. SSRF 三大入口：PDF 生成器、媒体处理（ffmpeg/ImageMagick/phantomjs）、URL 预览/webhook
2. 绕黑名单：gopher 打内网、DNS rebinding、open redirect 中转、AWS metadata 169.254.169.254
3. 升级链：SSRF→文件读→云凭证→RCE；LFI→session/日志 poison→RCE

### SQLi
1. 非常规入口：User-Agent、Host 头、忘记密码接口、上传字段名、INSERT 语句
2. 盲注变半盲（时间/布尔/DNS 外带）
3. `load_file`/`into outfile` 拿 shell 仍是有效终点

## 20 个最值得精读（按链式利用价值选）

1. [Self XSS to persistent XSS on login portal](https://medium.com/@nnez/always-escalate-from-self-xss-to-persistent-xss-on-login-portal-54265b0adfd0) — self-XSS 升级教科书
2. [Self XSS to Account Takeover](https://medium.com/@ch3ckm4te/self-xss-to-account-takeover-72c89775cf8f) — 升级 ATO 全链
3. [XSS to account takeover](https://noobe.io/articles/2019-10/xss-to-account-takeover) — XSS→ATO 最短路径
4. [Exploiting cookie based XSS by finding RCE](https://noobe.io/articles/2019-09/exploiting-cookie-based-xss-by-finding-rce) — cookie XSS 反打 RCE
5. [XSS to RCE](https://medium.com/@hungrybytes/xss-to-rce-in-e20b2bc55f94) — XSS 直升 RCE
6. [Reflected XSS with HTTP Smuggling](https://hazana.xyz/posts/escalating-reflected-xss-with-http-smuggling/) — 走私升级存储型
7. [Chaining cache poisoning to stored XSS](https://medium.com/@nahoragg/chaining-cache-poisoning-to-stored-xss-b910076bda4f) — 缓存投毒链
8. [Bypass Uppercase filters like a PRO](https://medium.com/@Master_SEC/bypass-uppercase-filters-like-a-pro-xss-advanced-methods-daf7a82673ce) — WAF 高级绕过
9. [5000 USD XSS at avast desktop antivirus](https://medium.com/bugbountywriteup/5-000-usd-xss-issue-at-avast-desktop-antivirus-for-windows-yes-desktop-1e99375f0968) — 桌面软件 XSS 高赏金
10. [Leveraging angularjs based XSS to privilege escalation](https://www.shawarkhan.com/2019/08/leveraging-angularjs-based-xss-to-privilege-escalation.html) — AngularJS 提权
11. [Blind XSS : a mind Game](https://medium.com/@dirtycoder0124/blind-xss-a-mind-game-to-win-the-battle-4fc67c524678) — 盲打思维
12. [How I was able to get private ticket response panel via blind XSS](https://pwnsec.ninja/2019/06/06/how-i-was-able-to-get-private-ticket-response-panel-and-fortigate-web-panel-via-blind-xss/) — 打穿 Fortigate 后台
13. [Story of a Full ATO N/A to Accepted](https://medium.com/@nandwanajatin25/story-of-a-stored-xss-to-full-account-takeover-vulnerability-n-a-to-accepted-8478aa5e0d8e) — 被拒后坚持升级
14. [Researching Polymorphic Images for XSS on Google Scholar](https://blog.doyensec.com/2020/04/30/polymorphic-images-for-xss.html) — 多态图片
15. [Unicode vs WAF](https://medium.com/bugbountywriteup/unicode-vs-waf-xss-waf-bypass-128cd9972a30) — Unicode 绕过
16. [XSS WAF Character limitation bypass like a boss](https://medium.com/bugbountywriteup/xss-waf-character-limitation-bypass-like-a-boss-2c788647c229) — 长度限制绕过
17. [XSS on Facebook instagram CDN bypassing signature](https://www.amolbaikar.com/xss-on-facebook-instagram-cdn-server-bypassing-signature-protection/) — CDN 签名绕过
18. [Exploiting websocket application wide XSS CSRF](https://medium.com/@osamaavvan/exploiting-websocket-application-wide-xss-csrf-66e9e2ac8dfa) — WebSocket 攻击面
19. [How Paypal helped me to generate XSS](https://medium.com/@pflash0x0punk/how-paypal-helped-me-to-generate-xss-9408c0931add) — 第三方借用
20. [How I found XSS by searching in shodan](https://blog.usejournal.com/how-i-found-xss-by-searching-in-shodan-6943b799e648) — 测绘引擎反查

## 给补天 SRC 新手的 5 条建议

1. **self-XSS 别直接交**，先想升级路径（登录 CSRF / clickjacking / 授权缺陷）
2. **Blind XSS 部署要全**：工单、表单、文件名、UA 全部埋点，等管理员上钩
3. **认证流全家桶是最值得深耕的洞型**：注册→重置→2FA→竞态，按 checklist 逐项过
4. **每个漏洞想一遍影响链**：单独低危的组合起来可能是 Critical（XSS→凭据→ATO；SSRF→凭证→RCE）
5. **403 不是终点**：换方法/换角色/换版本/换路径四种姿势各试一遍再放弃
