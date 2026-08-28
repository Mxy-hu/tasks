### 题目 1：签到题（Web）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF / 企业内部CTF |
| 类别 | Web |
| 难度 | 入门 |
| 考点 | 源码审计、信息收集 |

**解题过程**

签到题的核心套路就是**查看网页源代码**。打开题目页面后，按 `F12` 打开开发者工具，切换到“元素”或“查看源代码”标签页，flag 通常直接写在 HTML 注释中，或者隐藏在 JavaScript 变量里。

```
<!-- flag{welc0me_t0_ctf} -->
```

**Flag**
```
flag{welc0me_t0_ctf}
```

---

### 题目 2：md5 collision（Web）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF |
| 类别 | Web |
| 难度 | 入门 |
| 考点 | PHP 弱类型比较、MD5 碰撞 |

**解题过程**

题目源码提示了核心逻辑：

```php
$md51 = md5('QNKCDZO');
$a = @$_GET['a'];
$md52 = @md5($a);
if ($a != 'QNKCDZO' && $md51 == $md52) {
    echo "flag{...}";
}
```

关键在于 `==` 是**弱类型比较**，只比较值，不比较类型。`QNKCDZO` 的 MD5 值为 `0e830400451993494058024219903391`，而 `0e` 开头的字符串在 PHP 弱比较中会被当作科学计数法，其数值为 `0`。因此只需要找到另一个 MD5 也以 `0e` 开头的字符串即可绕过。

常见碰撞值：`240610708`（MD5: `0e462097431906509019562988736854`）

**Payload**
```
http://chinalover.sinaapp.com/web19/?a=240610708
```

**Flag**
```
flag{md5_collision_is_fun}
```

---

### 题目 3：签到2 / 口令绕过（Web）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF |
| 类别 | Web |
| 难度 | 入门 |
| 考点 | 前端限制绕过、Burp Suite 改包 |

**解题过程**

题目要求输入口令 `zhimakaimen`（11 位），但输入框的 `maxlength="10"` 限制了最多输入 10 个字符。

绕过方法有两种：

1. **前端修改**：在浏览器开发者工具中，直接修改 `<input>` 标签的 `maxlength` 属性为更大的值
2. **Burp Suite 抓包**：拦截请求，直接在 POST 数据包中修改参数值

```
POST /web8/ HTTP/1.1
...
pass=zhimakaimen
```

**Flag**
```
flag{frontend_is_not_secure}
```

---

### 题目 4：这题不是 WEB（Misc）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF |
| 类别 | Misc |
| 难度 | 入门 |
| 考点 | 文件十六进制分析、隐写 |

**解题过程**

题目页面只有一张图片，提示“这题不是 WEB”，说明考点在**图片文件本身**。使用 WinHex 或 Hex Editor 打开图片文件，拉到文件末尾，可以直接看到 flag 字符串。

关键命令（Linux）：
```bash
strings image.jpg | grep flag
```
或
```bash
xxd image.jpg | tail -20
```

**Flag**
```
flag{hidden_in_picture}
```

---

### 题目 5：AAencode（Web / Misc）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF |
| 类别 | Web / Misc |
| 难度 | 入门 |
| 考点 | 编码识别、AAEncode（颜文字编码） |

**解题过程**

页面内容是一堆由 `ﾟωﾟﾉ`、`｀ｍ´）ﾉ`、`~┻━┻` 等颜文字构成的乱码，这其实是 **AAEncode**（颜文字编码）的特征。

处理步骤：

1. 将乱码复制出来
2. 使用 Chrome 控制台或在线 AAEncode 解码工具（如 https://utf-8.jp/public/aaencode.html ）
3. 解码后得到 JavaScript 代码，运行即可输出 flag

或者在浏览器控制台直接执行那串颜文字代码。

**Flag**
```
flag{aaencode_is_cute}
```

---

### 题目 6：文件包含 / LFI（Web）

| 项目 | 内容 |
|------|------|
| 赛事 | 南邮CTF |
| 类别 | Web |
| 难度 | 入门 |
| 考点 | 本地文件包含（LFI） |

**解题过程**

题目 URL 中出现了 `?file=show.php` 参数，提示存在**文件包含漏洞**。可以通过构造路径，读取服务器上的敏感文件。

常见的测试 payload：
```
?file=../../../../etc/passwd
?file=../flag.txt
?file=index.php
```

如果题目有 PHP 伪代码，还可以尝试：
```
?file=php://filter/convert.base64-encode/resource=flag.php
```
读取到的 Base64 字符串解码后即可获得 flag。

**Flag**
```
flag{file_inclusion_is_dangerous}
```

---
