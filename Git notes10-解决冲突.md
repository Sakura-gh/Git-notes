# Git notes 解决冲突

### 主要是两条分支上内容无法自动合并的情况

* 重新创建一个新的分支

  ~~~bash
  $ git checkout -b feature1
  Switched to a new branch 'feature1'
  ~~~

* 修改`readme.txt`最后一行，改为

  ~~~bash
  Creating a new branch is quick AND simple
  ~~~

* 在`feature1`分支上提交

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'AND simple'
  [feature1 5cc3902] AND simple
   1 file changed, 2 insertions(+), 1 deletion(-)
  ~~~

* 切换到`master`分支

  ~~~bash
  $ git checkout master
  Switched to branch 'master'
  Your branch is ahead of 'origin/master' by 4 commits.
    (use "git push" to publish your local commits)
  ~~~

  `Your branch is ahead of 'origin/master' by 4 commits`告诉我们当前的`master`分支比远程的`master`分支要超前4个提交commits

* 在`master`分支上把readme.txt文件的最后一行改为

  ~~~bash
  Creating a new branch is quick & simple
  ~~~

* 在`master`分支上提交

  ~~~bash
  $ git add readme.txt
  $ git commit -m '& simple'
  [master 60b22ee] & simple
   1 file changed, 2 insertions(+), 1 deletion(-)
  ~~~

* 现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

  ![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

* 这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

  ~~~bash
  $ git merge feature1
  Auto-merging readme.txt
  CONFLICT (content): Merge conflict in readme.txt
  Automatic merge failed; fix conflicts and then commit the result.
  ~~~

  果然冲突了！Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

  ~~~bash
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 5 commits.
    (use "git push" to publish your local commits)
  
  You have unmerged paths.
    (fix conflicts and run "git commit")
    (use "git merge --abort" to abort the merge)
  
  Unmerged paths:
    (use "git add <file>..." to mark resolution)
          both modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

* 直接查看readme.txt的内容

  ~~~bash
  Git is a distributed version control system.
  Git is free software distributed under the GPL.
  Git has a mutable index called stage.
  Git tracks changes of files.
  ---
  <<<<<<< HEAD
  Creating a new branch is quick & simple.
  =======
  Creating a new branch is quick AND simple.
  >>>>>>> feature1
  ~~~

  Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

  ```bash
  Creating a new branch is quick and simple.
  ```

* 再次提交

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'conflict fixed'
  [master 69225f2] conflict fixed
  ~~~

* 现在，`master`分支和`feature1`分支变成了下图所示：

  ![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

* 用`git log --graph --pretty=oneline --abbrev-commit`可以看到分支的合并情况：

  ~~~bash
  $ git log --graph --pretty=oneline --abbrev-commit
  *   69225f2 (HEAD -> master) conflict fixed
  |\
  | * 5cc3902 (feature1) AND simple
  * | 60b22ee & simple
  |/
  * b4e3885 delete
  * 1bef006 ...
  * ab79a7c 还原
  * fa32456 branch test
  * 2300c5c (origin/master) add ---
  * 4cb786f remove test.txt
  * 851861b add test.txt
  * 252eaea remodified
  * d513833 git tracks changes
  * b3c44a0 understand how stage works
  * cfa8029 append GPL
  * b38a1db add distributed
  * d236a5c wrote a readme file
  ~~~

* 最后，删除`feature1`分支

  ~~~bash
  $ git branch -d feature1
  Deleted branch feature1 (was 5cc3902).
  ~~~

  ### 小结

  当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成

  解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交

  用`git log --graph`命令可以看到分支合并图

