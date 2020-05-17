---
layout: post
tags: Windows
---

开启Windows开发者选项

打开子系统功能

下载
`curl.exe -L -o ubuntu-1804.appx https://aka.ms/wsl-ubuntu-1804`

解压
`Add-AppxPackage .\app_name.appx`
没成功，最后我是用7zip直接解压的

安装
管理员运行`ubuntu.exe`

---

重启linux

`services.msc` -> 重启`LxssManager`服务

