# Git 提交干净的commit

[参考文章](https://zhuanlan.zhihu.com/p/83016300)


<img alt="标准分支图" src="https://pic4.zhimg.com/80/v2-3b511d6395dd377edd13a18c8b18ccd7_hd.jpg">

推荐使用命令：
```
  # 提交时合并commit
  git merge <branch> --squash

  # 修改最后一次提交
  git commit --amend

  # 合并多个commit历史
  git rebase -i HEAD~n
```
