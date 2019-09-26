# Git notes bug分支

* 软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除

* 当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

  ~~~bash
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 10 commits.
    (use "git push" to publish your local commits)
  
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          modified:   readme.txt
  ~~~

* 并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

  幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

  ~~~bash
  $ git stash
  Saved working directory and index state WIP on master: 0d38866 merge with no-ff
  ~~~

* 此时，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug

  ~~~bash
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 10 commits.
    (use "git push" to publish your local commits)
  
  nothing to commit, working tree clean
  ~~~

* 首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

  ~~~bash
  $ git checkout master
  Switched to branch 'master'
  Your branch is ahead of 'origin/master' by 10 commits.
    (use "git push" to publish your local commits)
    
  $ git checkout -b issue-101
  Switched to a new branch 'issue-101'
  ~~~

* 现在修复bug，在readme.txt上增加一行`deal the bug`，然后提交

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'fix bug 101'
  [issue-101 d2bb720] fix bug 101
   1 file changed, 2 insertions(+)
  ~~~

* 切换回master分支，合并修改，并删除临时分支

  ~~~bash
  $ git checkout master
  Switched to branch 'master'
  Your branch is ahead of 'origin/master' by 10 commits.
    (use "git push" to publish your local commits)
  $ git merge --no-ff -m 'merge bug fix 101' issue-101
  Merge made by the 'recursive' strategy.
   readme.txt | 2 ++
   1 file changed, 2 insertions(+)
  $ git branch -d issue-101
  Deleted branch issue-101 (was d2bb720).
  ~~~

* 然后回到dev分支继续干活

  ~~~bash
  $ git checkout dev
  Switched to branch 'dev'
  $ git status
  On branch dev
  nothing to commit, working tree clean
  ~~~

  查看工作区，发现是干净的

* 用`git stash list`命令查看之前存储的工作场景

  ~~~bash
  $ git stash list
  stash@{0}: WIP on master: 0d38866 merge with no-ff
  ~~~

* 工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

  一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

  另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

  ~~~bash
  $ git stash pop
  On branch dev
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  Dropped refs/stash@{0} (a30a017b392beb211ae1f565549dfc765458475d)
  ~~~

  此时再用`git stash list`查看，就看不到任何stash内容了

  ~~~bash
  $ git stash list
  
  ~~~

* 同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来

  为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

  ```bash
  $ git branch
  * dev
    master
  $ git cherry-pick 4c805e2
  [master 1d4b803] fix bug 101
   1 file changed, 1 insertion(+), 1 deletion(-)
  ```

  Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍

* ### 小结

  修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除

  当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场

  在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动