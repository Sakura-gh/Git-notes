# Git notes 多人协作

* 当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

* 要查看远程库的信息，用`git remote`，或者，用`git remote -v`显示更详细的信息：

  ~~~bash
  $ git remote
  origin
  $ git remote -v
  origin  git@github.com:BarasKing/learngit.git (fetch)
  origin  git@github.com:BarasKing/learngit.git (push)
  ~~~

* ### 推送分支

  推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

  ```bash
  $ git push origin master
  ```

  如果要推送其他分支，比如`dev`，就改成：

  ```bash
  $ git push origin dev
  ```

* ### 小结

  - 查看远程库信息，使用`git remote -v`；
  - 本地新建的分支如果不推送到远程，对其他人就是不可见的；
  - 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
  - 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
  - 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
  - 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突



