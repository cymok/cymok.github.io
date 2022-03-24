---
layout: post
tags: Git
---

git 子模块 递归 clone 

clone时带上递归参数 `--recursive`
```
git clone URL --recursive
```

---

如果 clone 时忘记带 `--recursive` 参数 可以后续初始化submodule
```
git submodule update --init
```

or  
代码克隆完成，需要初始化子模块。
```
git submodule init
git submodule update
```
