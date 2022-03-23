---
layout: post
tags: Linux
---

`apt autoremove` 一定要慎用

###### 原因

- 情况 1

在删除某些软件时，可能软件包本身被卸载了，但随着安装的依赖包没有被彻底清除，  
之后在 apt update 时，会提示使用 `apt autoremove` 来清理那些依赖包，  
可能 apt 不知道别的软件同时也依赖了，如果此时使用了前面的命令，会导致软件发生错误。  

- 情况 2 (网上查资料时看到别人的说法)

安装 c 的时候， c 依赖 b， b 又依赖 a，这个时候会自动安装 a 和 b  
但是，如果 c 利用了 b 默认隐含 a 包的条件，直接调用了 a 中的 api，  
而下次更新 b 包的时候，假如 b 不再依赖 a，这个时候 autoremove 会删掉 a 包，使 c 包不可用。  
于是当你 sudo apt remove c 后，autoremove 会删除 a, b  

###### 解决

- 尝试 apt install 重新安装 

- 查看 apt 的操作记录，尝试做个反向操作
```
vim /var/log/apt/history.log
```

###### 参考文章

[apt-get autoremove remove 新手收割者](https://blog.csdn.net/wesigj/article/details/108355649)  
[linux apt-get remove如何恢复](https://www.cnblogs.com/xiating/p/8810492.html)  
