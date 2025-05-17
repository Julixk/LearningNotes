# 用好你的电脑 II 课堂笔记

## 一、版本控制概览
### 1. 版本控制系统（VCS）
- **定义**：管理和追踪软件源文件版本的系统，提供协作、备份等功能。
- **分类**
    - **中心化**：需服务器存储所有版本记录，客户端拉取、修改并推送版本（如SVN）。
    - **去中心化**：每个客户端有完整版本记录，可通过中心服务器或客户端间交换信息同步版本（如Git）。
- **Git 与代码托管平台区别**：Git是VCS，GitHub、GitLab是基于Git的代码托管平台。

### 2. Git 的诞生背景
- 2005年，因Linux内核开发团队使用的BitKeeper授权被收回，Linus用一周时间开发了Git。

### 3. Git 的基本理念
- **抽象模型**：项目历史记录视为有向无环图（DAG），每个提交是节点，有一个或多个父节点，代表提交的衍生关系（如家谱）。
- **三个目录**
    - **工作区**：项目目录，可随意改动，记录修改前的区域。
    - **暂存区**：缓冲区，用于临时存放修改，以便一次性提交到版本库。
    - **版本库**：存储有向无环图的地方，保存所有提交记录。
- **工作流程**：修改文件→暂存修改（`git add`）→提交到版本库（`git commit`）。

## 二、Git 的基本使用
### 1. 初始化仓库
```bash
mkdir git-example  # 创建项目目录
cd git-example     # 进入目录
git init           # 初始化Git仓库
```

### 2. 配置 Git
- **全局配置**（用户名、邮箱、代理）
```bash
git config --global --edit  # 打开全局配置文件
```
在`[user]`模块下添加：
```ini
name = 你的用户名
email = 你的邮箱
```
在文件末尾添加代理配置（根据实际情况修改端口）：
```ini
[http]
    proxy = http://127.0.0.1:7890
[https]
    proxy = http://127.0.0.1:7890
```
- **局部配置**（针对单个仓库）
```bash
cd 仓库目录
git config --edit  # 编辑仓库本地配置
```

### 3. 暂存与提交更改
- **暂存文件**
```bash
echo "Hello, world!" > example1.txt  # 创建文件
git add .                            # 暂存所有未被忽略的文件
git add 文件名                        # 暂存指定文件
```
- **查看状态**
```bash
git status  # 查看当前分支状态
```
- **提交更改**
```bash
git commit -m "提交信息"  # 提交暂存区内容到版本库
```
- **补充**：使用`-a`参数可自动暂存修改过和删除的文件（不包含新文件）；使用`--amend`参数可修订上一个提交。

### 4. 查看与回退历史版本
- **查看提交历史**
```bash
git log  # 显示详细提交记录（含提交哈希值、作者、日期、提交信息）
```
- **回退到指定版本**
```bash
git checkout 提交哈希值  # 检出指定提交（进入分离头指针状态）
git checkout master     # 回到最新提交（master为主分支名）
git reset --hard 提交哈希值  # 强制回退到指定提交（谨慎使用，会丢失后续未提交修改）
```

### 5. 排除特定文件
- 创建`.gitignore`文件，写入要忽略的文件或文件夹规则
```bash
echo "confidential/" >> .gitignore   # 忽略confidential目录
echo "*.o" >> .gitignore             # 忽略所有.o文件
```
- 提交`.gitignore`文件
```bash
git add .gitignore
git commit -m "添加.gitignore"
```

## 三、分支管理
### 1. 创建分支
```bash
git checkout -b 分支名 提交哈希值  # 基于指定提交创建并切换到新分支（等价于先创建分支再切换）
```

### 2. 变基分支（Rebase）
- **作用**：将分支的提交记录移动到主分支的最新提交上，使分支历史更简洁。
- **命令**
```bash
git rebase master  # 将当前分支变基到master分支
git checkout master  # 切换回主分支
```
- **注意**：变基会重新计算提交的哈希值，谨慎在公共分支使用。

### 3. 合并分支与冲突解决
- **合并分支**
```bash
git checkout master  # 切换到目标分支（如master）
git merge 分支名     # 合并指定分支到当前分支
```
- **冲突解决**：当合并分支时同一文件同一行有冲突修改，Git会标记冲突
```plain
<<<<<<< HEAD       # 当前分支内容
Hello, Git2!
=======              # 分隔符
Hello, Git6!        # 待合并分支内容
>>>>>>> merge-example
```
手动修改冲突内容为期望结果，保存后执行：
```bash
git add .           # 暂存冲突解决方案
git merge --continue # 完成合并提交
```

## 四、多人合作与远程仓库
### 1. 克隆仓库
```bash
git clone url://仓库地址  # 复制远程仓库到本地
```

### 2. 拉取与推送代码
- **拉取代码**（更新本地代码）
```bash
git pull  # 等价于git fetch + git merge（或git rebase，取决于配置）
```
- **推送代码**（提交本地修改到远程仓库）
```bash
git push origin 分支名  # 推送当前分支到远程仓库（origin为默认远程仓库名）
git push --force       # 强制推送（谨慎使用，可能覆盖远程未拉取的提交）
```

### 3. 分叉与合并请求（PR/MR）
- **分叉**：无远程仓库写权限时，通过平台分叉功能创建独立仓库进行开发。
- **发起请求**：开发完成后，向原仓库发起合并请求，经审查后合并、修改或拒绝。
- **合并方式**：合并（Merge）、变基（Rebase）、压缩（Squash），根据需求选择（线性历史建议用变基或压缩）。

## 五、图形化工具
- **VSCode**：自带Git管理功能，支持可视化提交、冲突解决等。
- **gitg**：Gnome桌面环境下的Git图形化工具。
- **kommit**：KDE桌面环境下的Git图形化工具。

## 六、注意事项
- 谨慎使用`git reset --hard`和`git push --force`，避免丢失提交。
- 合理使用`.gitignore`排除敏感文件或编译产物。
- 多人合作时优先使用分支开发，通过合并请求审查代码。

## 七、参考资料
- [Git官方文档](https://git-scm.com/book)
- `git help`命令或`man git`查看详细手册。
