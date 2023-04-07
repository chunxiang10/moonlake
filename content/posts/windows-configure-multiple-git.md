---
title: 'windows配置多个git账号'
date: '2023-04-07T13:56:51+0800'
tags: []
categories: []
---

### 前言

我们为什么要配置多个git账号呢？

一般情况下，我们都是一台电脑配置一个git账号，使用如下命令：

```
git config --golbal user.name "user"
git config --golbal user.email "user@mail.com"
```

但是，如果我们的电脑里既有工作项目，又有个人项目，需要推送到不同的github仓库，该如何配置呢？

### 配置多个git账号

#### 移除git全局配置

1. 右键`Git Bash Here`打开Git命令行

2. 输入命令`git config --list`，查看全局配置

   ![](/pictures/multigit/multigit01.PNG)

3. 移除name、email、password等配置

   ```bash
   # 移除全局配置账户
   git config --global --unset user.name
   # 移除全局配置邮箱
   git config --global --unset user.email
   # 移除全局密码
   git config --global --unset user.password
   ```

#### 生成SSH KEY并在GitHub上部署

1. 右键`Git Bash Here`进入Git Bash，然后进入`.ssh`文件夹，生成SSH KEY。
   
   ```bash
   # 进入.ssh文件夹
   cd ~/.ssh
   # 生成user1的SSH KEY
   ssh-keygen -t rsa -C "user1@email.com"
   # 生成user2的SSH KEY
   ssh-keygen -t rsa -C "user2@email.com"
   ```

   ![](/pictures/multigit/multigit02.PNG)

2. 使用`ls`命令查看生成的4个密钥文件。使用cat命令查看公钥

   ```bash
   # 查看文件列表
   ls
   # 查看公钥
   cat user1_rsa.pub
   cat user2_rsa.pub
   ```
   ![](/pictures/multigit/multigit03.PNG)

1. 复制公钥，分别粘贴到对应的github的SSH key页面

   ![](/pictures/multigit/multigit04.PNG)

   ![](/pictures/multigit/multigit05.PNG)

#### 配置SSH的config文件

1. 在.ssh目录下创建一个`config.txt`文件，每个账号配置一个Host节点。

   ![](/pictures/multigit/multigit06.PNG)

   ```bash
   # 配置user1 
   Host user1.github.com
   HostName github.com
   IdentityFile C:\\Users\\lingh\\.ssh\\id_rsa
   PreferredAuthentications publickey
   User user1

   # 配置user2
   Host user2.github.com
   HostName github.com
   IdentityFile C:\\Users\\lingh\\.ssh\\id_rsa2
   PreferredAuthentications publickey
   User user2
   ```

   ![](/pictures/multigit/multigit07.PNG)

2. 然后将`config.txt`文件的`.txt`后缀删除

   ![](/pictures/multigit/multigit08.PNG)
   
3. 终端测试SSH Key是否生效

   ```bash
   ssh -T git@user1.github.com
   ssh -T git@user2.github.com
   ```

   ![](/pictures/multigit/multigit09.PNG)

#### 配置用户名和邮箱

为各仓库单独配置用户名和邮箱

```bash
git config user.name "user1"
git config user.email "user1@email.com"
```

如果原先使用HTTPS通信，则需要修改远程仓库地址

```bash
git remote rm origin
git remote add origin git@user1.github.com:user1/myproject.git
```

### 结语

我们在使用中，只需要为各个仓库单独配置用户名和邮箱，就可以实现在同一台电脑上，在不同的项目下，推送到不同的github仓库的功能。

---