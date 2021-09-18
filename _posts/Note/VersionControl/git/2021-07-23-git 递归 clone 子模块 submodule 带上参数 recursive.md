---
layout: post
tags: Git
---

git 子模块 递归 clone 

clone时带上递归参数 `--recursive`
```
git clone URL --recursive
```
如果clone时忘记带`--recursive`参数 可以后续初始化submodule
```
git submodule update --init
```
