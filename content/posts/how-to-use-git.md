---
title: 'git简单使用'
date: '2023-03-28T17:44:07+0800'
tags: []
categories: []
---
# git简单使用

1. 安装git

2. 注册github

3. 打开git bash

4. 切换到目录

5. 输入`git init`初始化目录

6. 本地创建SSH KEY

   ```bash
   ssh-keygen -t rsa -C "your_email@youremail.com"
   # your_email@youremail.com改为你在github上注册的邮箱
   # 之后按3下回车
   ```

7. 打开文件`C:\\Users\\Administrator\\.ssh\\id_rsa.pub`，复制里面的key

8. 打开github上，进入Settings（账户配置），左边选择SSH and GPG keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。

9. 验证是否连接成功`ssh -T git@github.com` 如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

10. 设置本地username和email

    ```bash
    git config --global user.name "your name"
    git config --global user.email "your_email@youremail.com"
    ```

11. 连接远程仓库 `git remote add origin git@github.com:yourName/yourRepo.git`

12. 更改master为main `git checkout -b main`

13. 删除master `git branch -d master`

14. 把项目所有文件加到缓存区 `git add .`(点为全部的意思)

15. 把缓存区里的文件提交到本地仓库`git commit -m '这里为注释，随便写'`

16. 合并add和commit `git commit -am "注释必填”`

17. 把仓库中的文件推送到github仓库`git push origin main`，可以把 main换成你想要推送的任何分支。

18. 如果新建仓库有readme.md文件，可以使用`git push -f origin main` 强制和本地仓库一致。

19. 版本退回

    `git log`查看历史记录信息

    `git reflog`获取版本号

    `git reset --hard HEAD^` 退回上一个版本

     `git reset --hard 版本号` 退回对应版本号

20. 排除不上传的文件

    ```bash
    # 新建 .gitignore 文件
    
    # 文件夹
    __pycache__/
    # 后缀包含c o d
    *.py[cod]
    *$py.class
    
    #django stuff
    local_settings.py
    *.sqlite3
    # database migrations
    */migrations/*.py
    !*/migrations/__init__.py
    ```