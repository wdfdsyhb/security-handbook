# 05 XSS 与 CSRF

> XSS 单独交大概率低危，值钱的是升级链：XSS → 窃取凭据 → ATO（账号接管）。290 条 XSS writeup 里近半是 self-XSS 升级链。

## XSS 基础分类与价值排序

- **存储型 >> 反射型**（影响大、赏金高）
- **盲 XSS（Blind XSS）**：payload 打进后台才触发——客服工单、联系表单、简历文件名、User-Agent、报错回执邮件，管理员打开后台时执行。工具：XSS Hunter / bXSS / ezXSS。**管理员面板的盲 XSS 价值最高**

## DOM XSS 审计方法

1. 找所有 `source`（用户可控输入）：`location.hash`、`location.search`、`document.URL`、`window.name`、`postMessage`
2. 追踪到 `sink`（危险 DOM 操作）：`innerHTML`、`document.write()`、`eval()`、`setTimeout()`、`$()`
3. 测试 source 是否无消毒到达 sink
4. 关键指标：**服务器永远看不到 payload**（#号后的内容不进 HTTP 请求）

## 过滤绕过

| 技术 | Payload |
|------|---------|
| 标签分割 | `<scr<script>ipt>alert(1)</scr<script>ipt>` |
| Unicode 转义 | `<script>\u0061lert(1)</script>` |
| 大小写变换 | `<ScRiPt>alert(1)</ScRiPt>` |
| SVG | `<svg onload=alert(1)>` |
| Markdown 注入 | `[a](javascript:prompt(document.cookie))` |
| CSS data URI | `background-image: url("data:image/jpg;base64,...")` |
| AngularJS 模板 | `{{constructor.constructor('alert(1)')()}}` |

## CSP 绕过

- JSONP 端点滥用：CSP 白名单域上的可利用 JSONP endpoint
- AngularJS 沙箱逃逸（旧版本）
- `base-uri` 操纵（CSP 未限制 base-uri 时注入 `<base>` 改相对路径解析）
- 文件与 Content-Type 入口：SVG/PNG content-type、MIME sniffing、polyglot 图片（多态图片绕图片渲染检测）、文件名、markdown 编辑器、Open Graph embed

## self-XSS 升级三件套（最高频套路）

1. **self-XSS + 登录页 CSRF** → 存储型 XSS：诱导用户在登录页提交含 payload 的表单
2. **self-XSS + Clickjacking**：诱导用户点击粘贴 payload
3. **self-XSS + 授权缺陷**：借 IDOR/越权把 payload 写进他人会话

## CSRF 攻击面（49 条 writeup 归纳）

- **价值标准 = 危害链**：改邮箱/密码的 CSRF → ATO 才是中危以上
- **JSON CSRF**：Content-Type/Method 覆盖绕过校验（`text/plain` 发 JSON、`_method=PUT` 覆盖）
- **Site-wide CSRF**：一处不校验 → 推断全站无防护
- **Token 缺陷四件套**：删 token 参数、token 不绑会话（A 的 token 给 B 用）、Referer 伪造（空 Referer 或子域）、GET/POST 双提交

## 打后台的位置（业务侧经验）

- 留言板/意见反馈：直接插 `<script>alert(1)</script>`，弹窗即中危
- **个人资料插 XSS 打管理员 cookie（命中率大）**：后台管理员查看用户列表/详情时触发
- 上传的 html 文件：目录不执行时传 html 打存储 XSS
- WebSocket 攻击面：ws 消息渲染点

## 影响升级思路

| 单点 | 升级链 |
|------|--------|
| 反射型 XSS | + HTTP 走私/缓存投毒 → 变存储型 |
| self-XSS | + 登录 CSRF/clickjacking/授权缺陷 → stored XSS |
| 存储型 XSS | → 窃取凭据 → ATO |
| cookie XSS | + 后台功能 → RCE（writeup 实证） |
| 管理员盲 XSS | → 后台操作 CSR F → 系统配置改写 |

## CSRF 防御视角

SameSite Cookie、CSRF Token 绑会话、校验 Content-Type、关键操作二次确认。**XSS 存在时 CSRF 防护全部失效**（XSS 能读 token）——修复优先级 XSS > CSRF。
