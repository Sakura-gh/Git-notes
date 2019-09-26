# Git notes 删除文件

* 删除文件有两种方式，一种是直接在文件管理器中把没用的文件直接删除，另一种是用命令行的方式即用`rm`命令删除

* 先在工作区新建一个test.txt文件，增加到暂存区并提交到本地仓库

  ~~~bash
  $ git add test.txt
  $ git commit -m 'add test.txt'
  [master 851861b] add test.txt
   1 file changed, 0 insertions(+), 0 deletions(-)
   create mode 100644 test.txt
  ~~~

* 然后直接用`rm`删除test.txt文件

  ~~~bash
  $ rm test.txt
  ~~~

* 删除后用`git status`查看一下工作区的状态

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          deleted:    test.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

  `Changes not staged for commit`说明暂存区中没有需要提交的内容

  ` deleted:    test.txt`说明工作区中删除了一个名叫test.txt的文件

* 此时有两个选择，第一个是用`git restore`撤回工作区的修改，另一个是直接用命令行在暂存区中删除

  ~~~bash
  $ git rm test.txt
  rm 'test.txt'
  ~~~

  已删除test.txt文件

* 此时再查看工作区状态

  ~~~bash
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          deleted:    test.txt
  ~~~

  说明在暂存区中删除了test.txt文件，但此时本地仓库里还是存在这个文件的

* 需要使用`git commit`来提交修改到本地仓库

  ~~~bash
  $ git commit -m 'remove test.txt'
  [master 4cb786f] remove test.txt
   1 file changed, 0 insertions(+), 0 deletions(-)
   delete mode 100644 test.txt
  ~~~

  此时已把本地仓库的test.txt文件删除

* 再次用`git status`查看工作区状态

  ~~~bash
  $ git status
  On branch master
  nothing to commit, working tree clean
  ~~~

  此时工作区、暂存区和本地仓库都是干净的

  

