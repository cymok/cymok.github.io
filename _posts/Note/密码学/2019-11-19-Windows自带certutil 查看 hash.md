---
layout: post
tags: Windows
---

利用Windows自带工具 `certutil` 查看文件Hash校验值 MD5 SHA1 SHA256

```
certutil -hashfile [FILE] MD5
```

```
certutil -hashfile [FILE] SHA1
```

```
certutil -hashfile [FILE] SHA256
```

* `[FILE]` 是带路径的完整文件名 或目录下或环境变量下的短文件名
