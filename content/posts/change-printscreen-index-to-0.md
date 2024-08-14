---
title: '将windowsPrintScreen的截图编号重置为1'
date: '2024-08-14T09:45:40+0800'
tags: []
categories: []
---


将以下内容复制到txt文件中，并将后缀改为`.rag`，双击运行即可充值

```powershell
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer]
"ScreenshotIndex"=dword:00000000
```

windows全屏截图快捷键：`win+PrintScreen`

保存位置：`C:\Users\admin\Pictures\Screenshots`