---
layout: post
tags: Git
---

## 基本使用

git submodule 管理子模块


直接添加一个submodule
```
git submodule add [URL] [SUBMODULE_NAME]
```

## 简单描述

执行上面命令后，会在根目录会生成一个 `.gitmodules` 文件
```
[submodule "[SUBMODULE_NAME]"]
	path = [SUBMODULE_PATH]
	url = [URL]
```

用于描述对应的子模块仓库

位于 path 的子模块仓库的 `.git` 是个文件，内容 `gitdir: ../.git/modules/xxx` 指向主仓库里，所以统一在主仓库的 `.git` 文件夹管理git

在主模块使用 `git status` 会发现

- 如果子模块的HEAD是主模块对应的commitId  
  - 在子仓库目录[SUBMODULE_PATH]添加一个文件，会由子模块仓库管理，而主仓库是干净的  

- 如果子仓库的HEAD不是主模块对应的commitId  
  - 子仓库目录[SUBMODULE_PATH]将是modified状态，内容类似是这样 `Subproject commit 1801682bb79741d6fc61a7bf7e4af41f0589a157`，实际是 子仓库的git管理文件 `gitdir: ../.git/modules/xxx/HEAD` 指向了 `1801682bb79741d6fc61a7bf7e4af41f0589a157` 这个 ref 引用

## 目录结构

- 主仓库只负责管理本身仓库和使用 `.gitmodules` 引用子仓库

- 子仓库 从 `.gitmodules` 对应的 url 找到远程仓库，对应本地的源码路径 path

```
[submodule "子仓库"]
	path = src/xx/xx
	url = xxx.git
```

然后从上面 path 目录的 `.git` 文件指向主仓库的 `.git` 目录 `主仓库/.git/modules/xxx`，接着从此目录的 `HEAD` 指向某个引用找到子仓库对应的源码
