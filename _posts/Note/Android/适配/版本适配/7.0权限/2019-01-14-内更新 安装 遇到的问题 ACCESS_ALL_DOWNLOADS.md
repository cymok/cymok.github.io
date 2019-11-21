---
layout: post
tags: Android 适配
---

`android.permission.ACCESS_ALL_DOWNLOADS`, or `grantUriPermission()`

下载完成安装时 调用安装器intent需要加

`Intent.FLAG_GRANT_READ_URI_PERMISSION`

`intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);`
