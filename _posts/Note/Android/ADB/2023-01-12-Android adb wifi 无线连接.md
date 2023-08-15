---
layout: post
tags: Android
---

Android 11+ 版本 无需数据线 支持直接在IDE和手机操作，以下是通用方法

# 手机端

## 无root设备 （缺点：需要先用数据线连接）

利用**数据线连接电脑**，打开adb默认端口监听
```
adb tcpip 5555
```

开启后就可以拔掉数据线用电脑连接进行无线调试了

## 有root设备

1. 有root权限 可以利用调试APP直接在手机端打开端口监听

2. 或者在手机端执行命令，利用超级终端（Terminal Emulator）
```
# root权限执行
su
# 设置系统属性 adb的tcp远程端口为5555
setprop service.adb.tcp.prot 5555
# 重启adb服务
stop adbd
start adbd
```

3. 或者写个APP，通过socket连接127.0.0.1:5555，判断设备adb的默认端口5555是否开启
```
int port = 5555;
Runtime.getRuntime().exec("su");
Runtime.getRuntime().exec("setprop service.adb.tcp.port " + port);
Runtime.getRuntime().exec("stop adbd");
Runtime.getRuntime().exec("start adbd");
```

4. 甚至可以将prop属性写到系统里，重启也能自动开启

    - 前提是root
    
    - 解锁system
    
    ```
    adb root
    adb disable-verity
    ```
    
    - 修改属性文件 `/default.prop` 或 `/system/build.prop` 增加属性
    
    ```
    setprop service.adb.tcp.prot 5555
    ```
    
    - system分区只读处理
    
    ```
    # 可读写
    mount -o rw,remount /system
    # 只读
    mount -o ro,remount /system
    ```

# 电脑端
```
# 连接
adb connect 192.168.31.211:5555

# 查看
adb devices
```
