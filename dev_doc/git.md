---
author: Lucian Yu
title: Git文档
---
# Git文档

## 一、Git常用指令

### 创建和初始化仓库

| Command                    | Description           |
| -------------------------- | --------------------- |
| git init                   | 初始化一个新的Git仓库 |
| git clone <repository_url> | 克隆远程仓库到本地    |
| git status                 | 查看当前仓库状态      |



### 配置Git

| Command                                               | Description     |
| ----------------------------------------------------- | --------------- |
| git config --global user.name "Your Name"             | 配置全局用户名  |
| git config --global user.email "mailname@example.com" | 配置全局邮箱    |
| git config --global --get user.name                   | 查看全局用户名  |
| git config --global --get user.email                  | 查看全局邮箱    |
| git config --list                                     | 查看Git配置信息 |



### 初始化新仓库

| Command                                                  | Description                    |
| -------------------------------------------------------- | ------------------------------ |
| git branch -M main                                       | 把当前分支改名为 main          |
| git remote add origin git@github.com:Varmice/myNotes.git | 连接远程仓库                   |
| git push -u origin main                                  | 把本地分支的提交推送到远程仓库 |



### 文件操作

| Command                          | Description                            |
| -------------------------------- | -------------------------------------- |
| git add <file_name>              | 添加单个文件至暂存区                   |
| git add .                        | 添加当前目录下所有修改过的文件至暂存区 |
| `git commit -m "Commit message"` | 提交更改到本地仓库                     |
| git rm <file_name>               | 删除文件从版本控制中移除               |



### 分支操作

| Command                                 | Description                  |
| --------------------------------------- | ---------------------------- |
| git branch -a                           | 查看当前所在分支             |
| git branch <branch_name>                | 创建新分支                   |
| git checkout <branch_name>              | 切换到指定分支               |
| git checkout -b <new_branch_name>       | 创建并切换到新分支           |
| git branch -d <branch_name>             | 删除本地分支（已合并的分支） |
| git branch -D <branch_name>             | 强制删除本地分支             |
| git branch -r                           | 查看所有远程分支             |
| git fetch origin                        | 拉去远端分支不合并           |
| git merge <branch_name>                 | 合并分支                     |
| git pull origin <branch_name>           | 拉去远程分支并合并           |
| git branch -vv                          | 查看本地分支与远程分支的绑定 |
| git branch --set-upstream-to=origin/xxx | 绑定当前分支到远端xxx分支    |



### 远程仓库操作

| Command                                                      | Description                  |
| ------------------------------------------------------------ | ---------------------------- |
| git remote -v                                                | 查看远程仓库信息             |
| git remote add origin ssh://git@your-server-ip:[port]/home/git/repos/git_test.git | 添加远程仓库                 |
| git pull origin <branch_name>                                | 拉取远程仓库的最新提交到本地 |
| git push origin <branch_name>                                | 推送本地分支到远程仓库       |
| git push -f origin master                                    | 强推到远端的master分支       |
| git push origin --delete <branch_name>                       | 删除远程分支                 |



### 查看历史记录

| Command                                  | Description                  |
| ---------------------------------------- | ---------------------------- |
| git log                                  | 查看提交历史                 |
| git log --oneline                        | 查看简洁的提交历史           |
| git log <file_name>                      | 查看某个文件的历史记录       |
| git log -n \<number>                     | 查看提交的历史并限制显示数量 |
| git diff <commit_hash_1> <commit_hash_2> | 查看两次提交之间的差异       |



### 回退操作

| Command                          | Description                    |
| -------------------------------- | ------------------------------ |
| git reset --soft <commit_hash>   | 回退到某个提交（保留更改）     |
| git reset --mixed <commit_hash>  | 回退到某个提交并更新工作目录   |
| git reset --hard <commit_hash>   | 彻底回退到某个提交（丢弃更改） |
| git reset                        | 回退到上一个提交               |
| git restore --staged <file_name> | 移出暂存区                     |
| git restore <file_name>          | 还原成上次提交(还没进入暂存区) |



### 其他常用命令



## 二、服务器配置Git

### 步骤1：安装Git

1. 在云服务器上安装Git
   `sudo yum install -y git`
2. 检查是否安装成功
   `git --version`

### 步骤2：创建单独的Git用户

1. 创建用户：

   `sudo adduser git`

2. 设置密码：

   `sudo passwd git`

3. 验证用户创建，若出现类似：`git:x:1001:1001::/home/git:/bin/bash`，则说明创建成功：

   `cat /etc/passwd | grep git`

### 步骤3：在服务器上创建裸仓库

1. 切换到git用户
   `su - git`

2. 创建一个用于存放Git仓库的目录并切换至该目录（例如`/home/git/repos`）
   `mkdir -p /home/git/repos`

   `cd /home/git/repos`

3. 创建裸仓库（`myproject.git`）
   `git init --bare myproject.git`

4. 赋予合适的权限
   `chown -R git:git /home/git/repos/myproject.git`
   
   `chmod -R 755 /home/git/repos/myproject.git`

### 步骤4：在本地计算机配置远程Git

1. 切换到本地项目目录中初始化Git（如果尚未初始化）
   `git init`

2. 添加远程仓库
   `git remote add origin ssh://git@your_server_ip:port/home/git/repos/myproject.git`

3. `git add .`

   `git commit -m "Initial commit"`

   `git push -u origin master`

### 步骤5：配置SSH免密登录

1. 在本地机器生成SSH密钥（如果没有）：

   `ssh-keygen -t rsa -b 4096 -C "your-email@example.com"`

2. 将公钥上传到服务器：

   `ssh-copy-id git@your_server_ip`

   或者手动复制：

   `cat ~/.ssh/id_rsa.pub | ssh git@your-server-ip 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'`

3. 在本地测试SSH连接：

   `ssh git@your_server_ip`

### 步骤6：在其他设备克隆仓库

1. 在其他设备上，可以直接克隆仓库：

   `git clone ssh://git@your_server_ip:port/home/git/repos/myproject.git my_project_name`

2. 接下来就可以正常`pull/push`代码了

   `git pull origin master`

   `git push origin master`

## 三、额外优化

1. 为了安全起见，限制git用户只能使用Git操作，不允许Shell登录

   `sudo usermod -s /usr/bin/git-shell git`

2. 后续回复shell访问

   `sudo usermod -s /bin/bash git`

3. 个人使用，回退版本后本地历史提交记录和服务器端冲突，导致无法同步至服务器

   下面代码会将本地的提交历史覆盖云端，谨慎使用，仅限个人使用

   `git push -f origin master`

## 四、实例

> 详细：查看之前版本内容，并考虑是否回退

1. 查看历史提交，找到对应的提交哈希（commit hash）

   `git log --oneline`

   类似输出：

   ```git
   abc1234 (HEAD -> master) Commit message for 2
   def5678 Commit message for 1
   ```

2. 临时查看之前版本内容

   `git checkout <对应哈希值>`

   `cat file` 查看文件内容

3. 若要回退最新提交并不对任何分支做任何修改

   `git checkout master`

   若要决定回退某个版本

   `git reset --hard/soft/mixed <对应版本的哈希值>`



## 五、清空commit历史

```bash
# 1. 创建一个临时分支（以当前文件为基础）
git checkout --orphan temp_branch

# 2. 添加所有当前文件
git add .

# 3. 创建一次新的初始提交
git commit -m "Initial commit (reset history)"

# 4. 删除原分支（例如 main）
git branch -D main

# 5. 重命名临时分支为 main
git branch -m main

# 6. 强制推送到远程（⚠️ 会覆盖远程历史）
git push -f origin main
```



## 六、拉取远端最新至本地

```bash
git fetch origin
git reset --hard origin/main
git clean -fd  # 可选，清理未跟踪文件
```



## 七、已有远程修改main为master

```bash
# 1. 确保在 main 分支上并拉取最新代码
git checkout main
git pull origin main

# 2. 创建并切换到 master 分支
git checkout -b master

# 3. 推送新的 master 分支到远程
git push origin master

# 4. 设置 master 为默认分支（在 GitHub/GitLab 等平台设置）
# 或者通过命令行设置上游分支
git push --set-upstream origin master

# 5. 删除远程的 main 分支（谨慎操作）
git push origin --delete main

# 6. 删除本地的 main 分支
git branch -d main

# 7. 更新本地分支跟踪
git branch --set-upstream-to=origin/master master
```

## 八、Clone的几种方式

```bash
# 1. 默认克隆（使用远程名称）
git clone ssh://git@47.121.130.33:2222/home/git/repos/backend-system.git
# 结果：创建本地目录 backend-system/

# 2. 自定义本地目录名
git clone ssh://git@47.121.130.33:2222/home/git/repos/backend-system.git my-project
# 结果：创建本地目录 my-project/

# 3. 克隆到当前目录
git clone ssh://git@47.121.130.33:2222/home/git/repos/backend-system.git .
# 结果：在当前目录初始化仓库
```



## 九、常见问题

### 中文编码问题

解决方法:配置 Git 输出编码为 UTF-8

```bash
git config --global core.quotepath false
```

Git 使用 UTF-8 编码

```bash
git config --global i18n.logOutputEncoding utf-8
git config --global i18n.commitEncoding utf-8
```

设置gitk字符集

```bash
git config --global gui.encoding utf-8
```

设置Git自动转换换行符为LF

```bash
# 设置Git自动转换换行符
git config --global core.autocrlf false
git config --global core.eol lf
```

