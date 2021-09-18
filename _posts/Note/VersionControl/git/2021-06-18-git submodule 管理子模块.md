---
layout: post
tags: Git
---

git submodule 管理子模块


直接添加一个submodule
```
git submodule add [URL] [SUBMODULE_NAME]
```

在根目录会生成一个 `.gitmodules` 文件
```
[submodule "[SUBMODULE_NAME]"]
	path = [SUBMODULE_PATH]
	url = [URL]
```

用于描述对应的子模块仓库

子模块仓库的 `.git` 是个文件 内容 `gitdir: ../.git/modules/xxx` 指向主仓库里 所以统一在主仓库的 `.git` 文件夹管理git

在主模块使用 `git status` 会发现

- 如果子模块的HEAD是主模块对应的commitId  
  - 在子仓库目录[SUBMODULE_PATH]添加一个文件，会由子模块仓库管理，而主仓库是干净的  

- 如果子仓库的HEAD不是主模块对应的commitId  
  - 主仓库会的子仓库目录[SUBMODULE_PATH]将是modified状态，内容类似是这样 `Subproject commit 1801682bb79741d6fc61a7bf7e4af41f0589a157`
  

