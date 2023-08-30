---
layout: post
tags: Git
---

合并分支 太多提交信息 但又不想要，想要压缩到一个提交再合并过来 

切换到被合并分支直接用 `git rebase -i` 然后选择逐个提交选择 squash, 可以实现目的, 但又太麻烦, 可以但没必要!

有另一个方案

从 feature/xxx 分支创建一个孤立分支 orphan,  
同时会将目标分支所有提交复制到新分支合并重新提交一次，也就是只有这一个提交，  
达到了 rebase 的 squash 压缩所有提交为一个提交的目的

```
git checkout --orphan squash feature/xxx
```

再切换到需要合并的分支去合并这个孤立分支(如果想将此提交连接上历史提交就再 rebase 一次)，这样就简单快捷很多了
