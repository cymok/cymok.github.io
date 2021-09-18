---
layout: post
tags: Windows
---


打开注册表
```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor
```

添加字符串值
```
key=AutoRun
value=D:\cmd_auto_run.bat
```

在对应路径新建bat脚本
`D:\cmd_auto_run.bat`
```
@echo off
echo 你好啊，靓仔
```

打开CMD就能见到效果了
```
Microsoft Windows [版本 10.0.19041.153]
(c) 2020 Microsoft Corporation. 保留所有权利。
你好啊，靓仔

C:\Users\Administrator>
```
