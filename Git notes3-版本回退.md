# Git notes-版本回退

* 版本控制系统通过git log查看我们commit的历史记录

  ~~~bash
  $ git log
  commit cfa8029a6b5c6391e6f229499bd9c241462a2fac (HEAD -> master)
  Author: Sakura <1712824638@qq.com>
  Date:   Sun Aug 25 12:45:36 2019 +0800`
  
  ​	append GPL --第三次commit
  
  commit b38a1dbe164811816459fcb99779f2aaf83e14d2
  Author: Sakura <1712824638@qq.com>
  Date:   Sun Aug 25 12:33:20 2019 +0800`
  
  ​	add distributed --第二次commit
  
  commit d236a5c1b043e4a8c796b67c5596b40dc6f115bd
  Author: Sakura <1712824638@qq.com>
  Date:   Sun Aug 25 11:07:00 2019 +0800`
  
  ​	wrote a readme file  --第一次commit
  ~~~

  加上`--pretty=oneline`参数可以使版本信息缩减到一行：

  ~~~bash
  $ git log --pretty=oneline
  cfa8029a6b5c6391e6f229499bd9c241462a2fac (HEAD -> master) append GPL
  b38a1dbe164811816459，fcb99779f2aaf83e14d2 add distributed
  d236a5c1b043e4a8c796b67c5596b40dc6f115bd wrote a readme file
  ~~~

  注意：前面的一大串字符是`commit id`（版本号）

* 在Git中，用`HEAD`表示当前版本，也就是最新的提交`cfa802...`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

* 可以使用git reset命令，进行版本回退

  ~~~bash
  $ git reset --hard HEAD^
  HEAD is now at b38a1db add distributed
  ~~~

* 用cat命令查看txt里的内容，的确已被还原到上一个版本

  ~~~bash
  $ cat readme.txt
  Git is a distributed version control system
  Git is free software
  ~~~

  用git log查看近几次的版本库状态，现在已回到第二个版本状态，最新的第三个版本虽然没有显示出来，但实际上是存在的，只是HEAD指针指向了第二个罢了

  ~~~bash
  $ git log
  commit b38a1dbe164811816459fcb99779f2aaf83e14d2 (HEAD -> master)
  Author: Sakura <1712824638@qq.com>
  Date:   Sun Aug 25 12:33:20 2019 +0800
  
  ​	add distributed
  
  commit d236a5c1b043e4a8c796b67c5596b40dc6f115bd
  Author: Sakura <1712824638@qq.com>
  Date:   Sun Aug 25 11:07:00 2019 +0800
  
  ​	wrote a readme file
  ~~~

  

* 有时候版本回退之后，还想回到未来的版本，此时必须要知道commit id，而`git reflog`指令可以查看所有的命令以及commit id

  ~~~bash
  $ git reflog
  cfa8029 HEAD@{2}: commit: append GPL
  b38a1db HEAD@{3}: commit: add distributed
  d236a5c (HEAD -> master) HEAD@{4}: commit (initial): wrote a readme file
  ~~~

  先回到最新的版本

  ~~~bash
  $ git reset --hard d236a
  HEAD is now at d236a5c wrote a readme file
  ~~~

  只需要输入commit id的前几位即可

* 用cat查看内容，的确是最新的第三个版本

  ~~~bash
  $ cat readme.txt
  Git is a version control system.
  Git is free software.
  ~~~

  再用reflog查看历史commit id，多了两条reset操作

  ~~~bash
  $ git reflog
  d236a5c (HEAD -> master) HEAD@{0}: reset: moving to d236a
  b38a1db HEAD@{1}: reset: moving to HEAD^
  cfa8029 HEAD@{2}: commit: append GPL
  b38a1db HEAD@{3}: commit: add distributed
  d236a5c (HEAD -> master) HEAD@{4}: commit (initial): wrote a readme file
  ~~~

  小结

  `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`

  穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本

  要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本