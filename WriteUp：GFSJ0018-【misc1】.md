
## 题目名称
GFSJ0018-【misc1】

## 题目类型
Misc（杂项）- 编码转换

## 解题思路
题目给出一串十六进制字符串，直接解码得到的是非 ASCII 字符。观察数值范围，发现每个字节均大于 0xa0（160），尝试每个字节减去 0xa0 后得到可读字符，拼接后获得 Flag。

## 使用工具
- Python 3

## 关键步骤

### 1. 十六进制解码
d4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9e1e6b3e3b9e4b3b7b7e2b6b1e4b2b6b9e2b1b1b3b3b7e6b3b3b0e3b9b3b5e6fd

### 2. 编写 Python 脚本解码
```python
hex_str = "d4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9e1e6b3e3b9e4b3b7b7e2b6b1e4b2b6b9e2b1b1b3b3b7e6b3b3b0e3b9b3b5e6fd"
data = bytes.fromhex(hex_str)
result = bytearray()
for b in data:
    result.append(b - 0xa0)
print(result.decode('ascii'))
python3 solve.py
hctf{4e5t1a8f4e5t1a8}
