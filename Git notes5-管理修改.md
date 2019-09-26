# Git notes 管理修改

* Git跟踪管理的是修改，而非文件，每一次commit只会提交暂存区里的内容，并不会提交工作区里的内容

* 再次修改readme.txt，增加`Git tracks changes`

* 然后用`$ git add readme.txt`将之添加到暂存区

* 用git status查看工作区的状态

  ~~~bash
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          modified:   readme.txt
  ~~~

  readme.txt已被修改

* 再次修改readme.txt，将`Git tracks changes`改为`Git tracks changes of files`

* 然后用`git commit`提交

  ~~~bash
  $ git commit -m 'git tracks changes'
  [master d513833] git tracks changes
   1 file changed, 2 insertions(+), 1 deletion(-)
  ~~~

* 提交后，用`git status`查看状态

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

  发现第二次的修改并没有被提交，实际上，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交

* 提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

  ~~~bash
  $ git diff HEAD -- readme.txt
  diff --git a/readme.txt b/readme.txt
  index 5f1bced..ed02ad6 100644
  --- a/readme.txt
  +++ b/readme.txt
  @@ -1,4 +1,4 @@
   Git is a distributed version control system
   Git is free software distributed under the GPL
   Git has a mutable index called stage
  -Git tracks changes
  \ No newline at end of file
  +Git tracks changes of files
  \ No newline at end of file`
  ~~~

* 小结

  每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中





