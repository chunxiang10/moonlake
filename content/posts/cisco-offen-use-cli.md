--- 
date: 2024-09-09T09:01:23+08:00
title: "思科交换机常用命令"
tags: []
categories: []
---


```bash
# 进入特权模式
switch>en
# 输入密码
Password:******
# 查看运行配置
show running-configure
# 进入配置模式
config terminal
# 进入端口
interface GigabitEthernet0/1
# 修改端口模式
switchport mode access
# 加入vlan
switchport access vlan 10
# 退出
end
# 保存
write

```