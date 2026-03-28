
# Git 提交流程说明：创建并推送 hello.md

## 1. 准备工作
- 确保已安装 Git 并配置好用户名和邮箱：
  ```bash
  git config --global user.name "你的姓名"
  git config --global user.email "你的邮箱"
确保本地仓库已关联远程仓库（例如 GitHub 上的仓库）。

## 2. 创建文件 hello.md
在本地仓库根目录下创建 hello.md 文件，内容为：
**Hello 明馨懿**
## 3. 查看当前状态
**'git status'**
## 4. 将文件添加到暂存区
**'git add'** hello.md
再次运行 git status，文件变为绿色，表示已进入暂存区。
## 5. 提交到本地仓库
**'git commit -m'** "添加 hello.md 文件"
此时已被记录成本地版本
## 6. 推送到远程仓库
**'git push -u origin main'**
推送成功后，可以在远程仓库（如 GitHub）中看到 hello.md 文件。

## 7. 验证
- 在远程仓库网页上查看文件是否存在且内容正确。
+ 拉取最新内容到本地另一位置。
