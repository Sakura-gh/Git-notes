# Git notes-修改删操作

* 修改readme.txt文件

  ~~~bash
  Git is a distributed version control system
  Git is free software 
  ~~~

  使用`git status`命令查看仓库当前状态

  输入 `git status` ， 结果如下：

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

  

  上面的命令输出告诉我们，`readme.txt`被修改过了，但`Changes not staged for commit`表明修改后的内容并不能被提交到本地仓库

* git diff 命令可查看具体修改的内容

  输入`git diff readme.txt`，结果如下：

  ~~~bash
  $ git diff readme.txt
  diff --git a/readme.txt b/readme.txt
  index d8036c1..be836b4 100644
  --- a/readme.txt
  +++ b/readme.txt
  @@ -1,2 +1,2 @@`
  -Git is a version control system. --原先版本
  -Git is free software. 
  \ No newline at end of file
  +Git is a distributed version control system  --新版本
  +Git is free software
  \ No newline at end of file
  ~~~

  

  通过git diff查看完修改后，即可提交修改：

  ~~~bash
  $ git add readme.txt -- 提交到暂存区
  ~~~

  在提交到本地仓库之前，先用git status查看一下仓库当前的修改状态

  ~~~bash
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          modified:   readme.txt
  ~~~

  

  其中`Changes to be committed`表明修改后的版本可以被提交到本地仓库

* ~~~bash
  git commit -m 'add distributed' --提交到本地仓库
  ~~~

* 然后再用git status查看此时的仓库状态

  ~~~bash
  $ git status
  On branch master
  nothing to commit, working tree clean
  ~~~

  当前已经没有需要提交的修改，且工作目录是干净的

* 小结

  要随时掌握工作区的状态，使用`git status`命令。

  如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。













