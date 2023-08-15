---
layout: post
tags: Git
---

#### git clone 时 把子模块也 clone 下来

git 子模块 递归 clone 

clone时带上递归参数 `--recursive`
```
git clone URL --recursive
```

#### 如果 clone 时忘记带 `--recursive` 参数 可以后续初始化 submodule
```
git submodule update --init
```

or  
代码克隆完成，需要初始化子模块。
```
git submodule init
git submodule update
```

#### reset 更新子模块到某个节点
先cd到submodule, 正常操作reset
```
cd submodule_dir
git reset --hard commit_id
```

#### 遍历 放弃子模块工作区的变动 submodule discard
```
git submodule foreach git reset --hard
git submodule foreach --recursive git reset --hard
```
