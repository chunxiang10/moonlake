---
date: '2024-10-10T10:08:22+08:00'
title: 'Linux防火墙配置IP禁止访问外网'
tags: ["Linux"]
categories: ['技术']
---

# Linux防火墙配置IP禁止访问外网

```bash
iptables -I FORWARD  -m mac --mac-source E4:DD:66:82:DE:F4 -j DROP
iptables -I FORWARD -s 10.0.0.12 -d 10.0.0.0/16 -j ACCEPT
iptables -I FORWARD -s 10.0.0.0/16 -d 10.0.0.12 -j ACCEPT
iptables -I FORWARD -s 10.0.0.12 -j REJECT
```