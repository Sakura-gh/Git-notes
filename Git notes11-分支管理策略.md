# Git notes 分支管理策略

* 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息

  如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

* 首先，先使目前只剩下master分支，然后重新创建一个dev分支

  ~~~bash
  $ git checkout -b dev
  Switched to a new branch 'dev'
  ~~~

* 修改readme.txt文件

  ~~~
  Git is a distributed version control system
  Git is free software distributed under the GPL
  Git has a mutable index called stage
  Git tracks changes of files
  ---
  Creating a new branch is quick and simple
  test
  ==================》》》》》》》》》》
  Git is a distributed version control system
  Git is free software distributed under the GPL
  Git has a mutable index called stage
  Git tracks changes of files
  ~~~

* 提交新的commit

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'add merge'
  [dev 7df6dc4] add merge
   1 file changed, 1 insertion(+), 3 deletions(-)
  ~~~

* 切换回master分支

  ~~~bash
  $ git checkout master
  Switched to branch 'master'
  ~~~

* 合并dev分支，使用`--no-ff`参数，表示禁用`Fast forward`

  ~~~bash
  $ git merge --no-ff -m 'merge with no-ff' dev
  Merge made by the 'recursive' strategy.
   readme.txt | 4 +---
   1 file changed, 1 insertion(+), 3 deletions(-)
  ~~~

  **因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去**

* 合并后，用`git log`看看分支历史

  ~~~bash
  $ git log --graph --pretty=oneline --abbrev-commit
  *   0d38866 (HEAD -> master) merge with no-ff  #不使用fast forward模式
  |\
  | * 7df6dc4 (dev) add merge
  |/
  
  * 290142b add merge  #使用fast forward模式
  
  *   69225f2 conflict fixed
  |\
  | * 5cc3902 AND simple
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
  :
  ...
  ~~~

* 可以看到，不使用`Fast forward`模式，merge后就像这样：

  ![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

* ### 分支策略

  在实际开发中，我们应该按照几个基本原则进行分支管理：

  首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

  那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

  你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了

  所以，团队合作的分支看起来就像这样：

  ![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

  ### 小结

  Git分支十分强大，在团队开发中应该充分应用

  合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并