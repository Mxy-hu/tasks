# CTF 与常见编码入门

## 前言

在 CTF（Capture The Flag）竞赛中，**编码** 是 Misc（杂项）和 Crypto（密码学）方向最基础的考点。掌握常见编码的识别与解码方法，是解决大量题目的前提。

本文档整理了 CTF 中最常出现的编码类型、特征及解码方法，供初学者参考。

---

## 一、编码 vs 加密

| 类型 | 特点 | 是否需要密钥 |
|------|------|-------------|
| 编码 | 可逆转换，通常无密钥 | 否 |
| 加密 | 需要密钥才能还原 | 是 |

> CTF 中很多“看起来像乱码”的字符串，实际上只是某种编码。

---

## 二、常见编码速查表

| 编码名称 | 特征 | 解码工具/方法 |
|----------|------|----------------|
| **Base64** | 字符集 `A-Za-z0-9+/=`，长度为 4 的倍数，末尾常有 `=` | `base64 -d`（Linux）、CyberChef |
| **Base32** | 字符集 `A-Z2-7=`，长度为 8 的倍数 | `base32 -d`、CyberChef |
| **Base16（Hex）** | 字符集 `0-9A-F`，长度为偶数 | `xxd -r -p`、CyberChef |
| **ASCII** | 可见字符（空格、数字、字母、标点） | `ord()` / `chr()` |
| **URL 编码** | 形如 `%20`（% + 两位十六进制） | `urldecode`、Python `urllib.parse.unquote()` |
| **Unicode 编码** | 形如 `\u4e2d` 或 `&#20013;` | Python `encode/decode` |
| **摩斯密码** | 仅包含 `.` `-` 和空格 | 摩斯密码表、在线工具 |
| **凯撒密码** | 字母表循环移位，频率分析可破 | `tr` 命令、Python 暴力移位 |
| **ROT13** | 凯撒移位 13 位（两次 ROT13 复原） | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| **Atbash** | 字母表反转（A↔Z，B↔Y...） | `tr 'A-Za-z' 'ZYX...Azyx...a'` |
| **二进制** | 仅包含 `0` 和 `1` | `int(bin_str, 2)` 转整数再转字符 |
| **八进制** | 仅包含 `0-7`，常用于 Linux 权限 | `int(oct_str, 8)` |
| **Brainfuck** | 仅包含 `<>+-[].,` | 在线解释器 |
| **JSFuck** | 仅包含 `[]()!+` | 浏览器控制台执行 |
| **Quoted-printable** | 形如 `=E5=BC=80` | `quopri.decode()` |
| **UUencode** | 以 `begin` 开头，`end` 结尾 | `uudecode` |

---

## 三、识别方法

### 3.1 观察字符集

```python
# 判断字符串字符集特征
def detect_charset(s):
    import string
    if all(c in string.hexdigits for c in s) and len(s) % 2 == 0:
        return "可能是 Hex"
    if all(c in string.ascii_letters + string.digits + '+/=' for c in s):
        return "可能是 Base64"
    if all(c in '01' for c in s):
        return "可能是二进制"
    if all(c in '.- ' for c in s):
        return "可能是摩斯密码"
    # ... 更多判断
    return "未知"
```
## 四、实用工具
### 4.1命令行工具
# Base64
echo "aGVsbG8=" | base64 -d

# Hex
echo "68656c6c6f" | xxd -r -p

# URL 解码
echo "hello%20world" | python3 -c "import sys; from urllib.parse import unquote; print(unquote(sys.stdin.read()))"

# ROT13
echo "uryyb" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# 字符转 ASCII 码
printf '%d\n' "'A"   # 输出 65
### 4.2python脚本模板
# 通用解码模板
import base64
from urllib.parse import unquote

# Hex 解码
hex_str = "68656c6c6f"
bytes.fromhex(hex_str).decode()

# Base64 解码
b64_str = "aGVsbG8="
base64.b64decode(b64_str).decode()

# URL 解码
url_str = "hello%20world"
unquote(url_str)

# 循环尝试多种编码
def try_decodes(s):
    print(f"原始: {s}")
    try:
        print(f"Hex: {bytes.fromhex(s).decode()}")
    except: pass
    try:
        print(f"Base64: {base64.b64decode(s).decode()}")
    except: pass
    
## CTF常见套路
### 5.1多重编码
原始Flag-Base64-Hex-题目给出
### 5.2图片隐写中的编码
* **LSB**隐藏二进制数据-转Ascll
* **像素RGB值**-转Ascll
### 5.3编程与压缩结合
先编程-再压缩
解法：先解码-发现\x1f\x8b-再用zlib.decompress()
### 5.4编码藏在文件/注释中
* **图片Exif信息**
* **文件末尾的附加数据**
* **用binwalk、string命令发现隐藏编码**
## 七、总结

| 关键点 | 说明 |
| :--- | :--- |
| **识别优先** | 先观察字符集，判断编码类型 |
| **逐层解码** | CTF 常有多层编码，一层层剥开 |
| **熟练工具** | CyberChef + Python + Linux 命令 |
| **积累经验** | 多做题，熟悉各类编码的“长相” |
