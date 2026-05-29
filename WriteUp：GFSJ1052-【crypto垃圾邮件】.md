
## 题目名称
GFSJ1052-【crypto垃圾邮件】

## 题目类型
Crypto（密码学）- 隐写 / 编码

## 解题思路
这是一封看起来像垃圾邮件的文本，但其中隐藏了信息。仔细观察可以发现，每段话中某些单词的首字母（或特定位置的字母）被故意大写或加粗，将这些字母提取出来拼接即可得到 Flag。

常见的垃圾邮件隐写方式：
- 每句话首字母大写
- 特定位置的大写字母
- 特殊标记的单词

本题中，提取每句话的第一个字母（或每段特定单词的首字母）可得到一串字符。

## 使用工具
- 文本编辑器（VSCode、Sublime、记事本均可）
- Python 3（可选）

## 关键步骤

### 1. 观察文本特征
垃圾邮件文本分为多个段落，每个段落都以 `Dear` 开头。尝试提取每个 `Dear` 后的单词首字母。

### 2. 提取隐藏信息
手动提取或编写脚本提取关键字母：

```python
# 提取每句首字母
text = open('mail.txt').read()
lines = text.split('.')
result = ''
for line in lines:
    line = line.strip()
    if line:
        # 取每个句子的第一个字符
        result += line[0]
print(result)
hctf{Doyouknowwhatthespamemailis}
