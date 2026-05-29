
## 题目名称
GFSJ0249-【misc_pic_again】

## 题目类型
Misc（杂项） - 图片隐写、文件分离、LSB隐写

## 解题思路
这是一道典型的“套娃”题，核心思路是层层剥茧：
1. 提供的图片本身是“载体”，其像素通道的 LSB（最低有效位）中隐藏了另一个文件。
2. 通过分析提取出的数据，发现其文件头为 `PK`（ZIP 文件头），将其保存为压缩包。
3. 解压压缩包后，得到一个无后缀的文件。通过分析其文件头，识别出它是一个 Linux 下的 ELF 可执行文件。
4. 在 ELF 文件中直接搜索 Flag 格式字符串 `hctf{`，即可得到最终答案。

## 使用工具（Linux 环境）
- **zsteg**：检测和提取 LSB 隐写数据
- **file**：识别文件类型
- **unzip**：解压 ZIP 文件
- **strings + grep**：从二进制文件中搜索可读字符串

## 关键步骤

### 1. 使用 zsteg 检测并提取隐藏的 ZIP 文件
```bash
# 查看图片中是否存在 LSB 隐写数据
zsteg -a misc_pic_again.png

# 提取 LSB 中隐藏的数据，保存为 zip 文件
zsteg -e b1,rgb,lsb,xy misc_pic_again.png > 1.zip
