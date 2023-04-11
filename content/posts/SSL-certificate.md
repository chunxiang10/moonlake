---
title: 'CloudFlare配合腾讯云申请SSL证书'
date: '2023-04-11T17:24:56+0800'
tags: []
categories: []
---

### 前言

我们在搭建个人网站之后，又申请了域名，最后一步就是给我们的网站申请SSL证书，使用https协议，让我们的网站更安全。

申请免费SSL证书的主要方式是从Let‘s Encrypt申请，但是缺点也很明显，需要每隔90天重新一次，虽然可以使用脚本自动操作，单仍稍显麻烦。

腾讯云和阿里云都提供了申请免费SSL证书的功能，下面，我们使用腾讯云来申请免费的SSL证书，**持续时间一年**

### 申请证书

登陆[腾讯云控制台](https://console.cloud.tencent.com/)，搜索SSL证书

![](/pictures/ssl/ssl01.png)

选择左侧`我的证书`，点击`申请免费证书`

![](/pictures/ssl/ssl02.png)

![](/pictures/ssl/ssl03.png)

填写相关信息。一次只能绑定一个二级域名，如果要绑定泛域名(如：`*.xxx.com`)，需要付费。

![](/pictures/ssl/ssl04.png)

提交后出现如下界面

![](/pictures/ssl/ssl05.png)

此时，我们打开Cloud Flare，进入`dayue.love`站点，选择左侧的DNS，增加一个DNS记录。

![](/pictures/ssl/ssl06.png)

添加完成并保存后，回到腾讯云，点击验证。如果验证失败，可以多等一会再验证。

![](/pictures/ssl/ssl07.png)

收到审核通过的邮件后，回到腾讯云的SSL证书页面，点击左侧我的证书，找到我们绑定的域名，点击下载

![](/pictures/ssl/ssl08.png)

下载Nginx版本

![](/pictures/ssl/ssl09.png)

解压下载的zip文件，将文件夹内的`.key`和`.crt`文件上传到服务器

![](/pictures/ssl/ssl10.png)

```bash
# 进入nginx文件夹
cd /etc/nginx
# 新建cert文件夹
mkdir cert
# 将文件上传到cert文件夹
ls
test.dayue.love_bundle.crt
test.dayue.love.key
```

![](/pictures/ssl/ssl11.png)

打开`nginx.conf`，添加一个新server：

```bash
server{
     listen 9004 ssl;
     server_name test;
     ssl_certificate cert/test.dayue.love_bundle.crt;
     ssl_certificate_key cert/test.dayue.love.key;
     ssl_session_timeout 5m;
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
     ssl_prefer_server_ciphers on;
     location / {
     # 转发docker的9010端口
     proxy_pass http://127.0.0.1:9010;
     	}
	}
```

等待DNS更新后，就可以使用`https://test.dayue.love`访问网站了

---

