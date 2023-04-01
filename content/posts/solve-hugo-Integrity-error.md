---
title: '解决 Hugo Failed to Find a Valid Digest in the 'Integrity' Attribute错误'
date: '2023-04-01T20:55:40+0800'
draft: true
tags: []
categories: []
---

将hugo部署到CloudFlare的Pages服务器上，在本地打开时

将Hugo的博客环境从Mac的电脑转移到Win上面，结果样式不对，报错如下：

```bash
Failed to Find a Valid Digest in the 'Integrity' Attribute
```
Google 了好半天，发现是因为Mac和Win中的文件使用了不同的结尾符导致计算的hashcode不一样导致的。

解决方案
我的方法比较粗暴，就是将整个Repo转换成LF换行的，问题解决。

转换方法
将一个repo强制使用LF换行的方法。

添加.gitattributes文件
在项目根目录添加一个名为.gitattributes的文件，里面内容为：
```
* text=auto eol=lf
```
然后将这个修改commit一下 。

修改所有的文件
项目中之前文件可能已经使用了CRLF了，使用如下命令转换一下：
```
# Remove every file from git's index.
git rm --cached -r .
# Rewrite git's index to pick up all the new line endings.
git reset --hard
```
这样就可以保证项目中所有的文件都使用LF换行了。