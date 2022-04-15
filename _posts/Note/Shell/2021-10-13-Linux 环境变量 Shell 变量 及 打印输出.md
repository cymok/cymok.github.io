---
layout: post
tags: Linux
---

变量可以分为两大类，环境变量和shell变量

**环境变量**是系统范围内可用的变量，由生成的子进程和shell继承

**Shell变量**是仅适用于当前shell实例的变量。每个shell如zsh和bash，都有自己的一组内部shell变量

有几个命令可用于在Linux中列出和设置环境变量
- `env` - 该命令允许您在自定义环境中运行另一个程序，而无需修改当前程序。在没有参数的情况下使用时，它将打印当前环境变量的列表。
- `printenv` - 该命令打印所有或指定的环境变量。
- `set` - 该命令设置或取消设置shell变量。在没有参数的情况下使用时，它将打印所有变量的列表，包括环境和shell变量以及shell函数。
- `unset` - 该命令删除shell和环境变量。
- `export` - 该命令设置环境变量。

---

```
# 打印输出环境变量列表
printenv

# 打印输出指定环境变量, 可输出多个
printenv JAVA_HOME ANDROID_HOME

# 打印输出指定环境变量
echo $JAVA_HOME
```
