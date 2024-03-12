--- 
date: 2024-03-12T15:26:32+08:00
title: "Docker连接本地PostgreSql数据库"
authors: []
tags: ['docker']
categories: []
---
1. 修改PostgreSql配置文件
	```bash
	# /etc/postgresql/<版本号>/main/postgresql.conf
	listen_addresses = '*'
	```
	
2. 修改pg_hba.conf
	```bash
	# /etc/postgresql/<版本号>/main/pg_hba.conf
	# 增加以下内容
	host       all     all      172.17.0.0/16      password
	```
3. 重启数据库
	```bash
	systemctl restart postgresql<版本号>.service
	```
