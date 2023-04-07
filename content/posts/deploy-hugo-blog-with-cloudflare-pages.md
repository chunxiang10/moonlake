---
title: '使用Cloud Flare托管你的博客'
date: '2023-04-06T16:03:11+0800'
tags: []
categories: []
---

### 前言

我们在本地搭建好博客之后，需要将博客放在公网的云服务器上，让所有人都可以访问。国内的云服务器运营商收费都比较高，那么有没有免费的云服务托管我们的博客呢？答案是有。

Cloud Flare最近推出了Pages的功能，可以**免费**托管我们的静态博客，我们一起来看看如何设置吧~
### 准备工作

- Github账号
- Cloud Flare账号
- 一个自己的域名

### 创建Github仓库

登陆Github，创建一个新的仓库。Public或者Private都可以。

### 设置Cloud Flare Pages

1. 登陆Cloud Flare，点击左侧的Pages，然后创建项目

   ![cfp01](/pictures/cfp/cfp01.png)

2. 选择连接到Git

   ![cfp02](/pictures/cfp/cfp02.png)

3. 连接GitHub

   ![cfp03](/pictures/cfp/cfp03.png)

4. 输入GitHub的账号密码，点击Sign in

   ![cfp04](/pictures/cfp/cfp04.png)

5. 选择需要同步的仓库，完成授权

   ![cfp05](/pictures/cfp/cfp05.png)

6. 自动跳转回Cloud Flare后，选择GitHub账户，选择已授权的仓库

   ![cfp06](/pictures/cfp/cfp06.png)

7. 填入相关设置，**环境变量可以使用`hugo version`查看你自己HUGO的版本**，我的是0.111.3

   ![cfp07](/pictures/cfp/cfp07.PNG)

8. 等待片刻就部署完成了，我的项目总共用时23秒。然后我们就可以访问`zhaopin.pages.dev`查看我们的网站了。但是由于众所周知的原因，我们需要设置自己的域名进行跳转才能正常访问。

   ![cfp08](/pictures/cfp/cfp08.png)

### 设置自定义域名

1. 点击自定义域，设置自定义域

   ![cfp09](/pictures/cfp/cfp09.png)

2. 输入我们托管在Cloud Flare的域名

   **域名必须托管在Cloud Flare，否则无法设置自定义域。**

   ![cfp10](/pictures/cfp/cfp10.png)

3. 点击激活域，等待DNS记录更新，然后就可以用新的域名访问你的博客啦~

   ![cfp11](/pictures/cfp/cfp11.png)

---