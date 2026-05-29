## 题目名称
Base64 签到题
我发现了一段神秘的字符串，看起来像是 Flag 被编码了，你能帮我恢复它吗？
密文：aGN0ZntiYXNlNjRfaXNfZWFzeV9yaWdodH0=
## 题目类型
Misc(杂项)-编码
## 解题思路
字符串以'='结尾，字符集包括'A-Za-z0-9+/',符合Base64编码特征，直接解码即可。
## 使用工具
-linux命令行
-python
## 关键步骤
```bash
echo "aGN0ZntiYXNlNjRfaXNfZWFzeV9yaWdodH0=" | base64 -d
```
## 最终Flag
hctf{base64_is_easy_right}
