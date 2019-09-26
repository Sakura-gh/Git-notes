# Git notes 创建与合并分支

### 图解：

* 每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支，在Git里，有一个默认生成的分支叫主分支，即`master`分支

* 一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

  ![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

* 当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

  ![git-br-create](https://www.liaoxuefeng.com/files/attachments/919022363210080/0)

* 从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

  ![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/0)

* Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

  ![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

* 合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

  ![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)

### 实战演示：

* 首先，用`git branch dev`创建`dev`分支，然后用`git checkout dev`切换到`dev`分支

  ~~~bash
  $ git branch dev
  $ git branch
    dev
  * master
  ~~~

  创建新的分支dev，用`git branch`查看当前分支，其中`git branch`命令会列出所有分支，当前分支前面会标一个`*`号，上图表示当前分支还是在master

* 使用`git checkout dev`将`HEAD`切换到dev分支

  ~~~bash
  $ git checkout dev
  Switched to branch 'dev'
  $ git branch
  * dev
    master
  ~~~

  将`HEAD`切换到dev，然后用`git branch`查看当前分支，已经变成了dev

* 当然有一个操作可以一步实现创建新的dev分支，并切换到dev上面

  ~~~bash
  $ git checkout -b dev
  Switched to a new branch 'dev'
  $ git branch
  * dev
    master
  ~~~

* 在dev分支上正常修改提交，如对readme.txt删除最后一行：

  ~~~bash
  Creating a new branch is quick.
  ~~~

  提交修改：

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'delete'
  [dev b4e3885] delete
   1 file changed, 1 insertion(+), 2 deletions(-)
  ~~~

  此时dev分支的最后一行已消失，重新切换回master查看

  ~~~bash
  $ git checkout master
  Switched to branch 'master'
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)
  ~~~

  查看readme.txt发现原来的最后一行还存在，因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变

* 现在，我们用`git merge`命令把`dev`分支的工作成果合并到`master`分支上（注意必须先切换回`master`分支才能把`dev`分支合并到`master`上）：

  ~~~bash
  $ git merge dev
  Updating fa32456..b4e3885
  Fast-forward
   readme.txt | 3 +--
   1 file changed, 1 insertion(+), 2 deletions(-)
  ~~~

  此时再查看`master`分支的readme.txt，最后一行已被删除

  `git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的

* 注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

  当然，也不是每次合并都能`Fast-forward`

* 合并完成后，就可以删除dev分支了，用`git branch`查看，只剩下了`master`分支

  ~~~bash
  $ git branch -d dev
  Deleted branch dev (was b4e3885).
  $ git branch
  * master
  ~~~

### 小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`