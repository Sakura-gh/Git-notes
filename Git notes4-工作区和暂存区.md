# Git notes-工作区和暂存区

* 类似于learngit文件夹就是一个工作区

* 工作区的隐藏目录`.git`是Git的版本库，版本库里有很多东西，其中最重要的是`stage`（或叫做`index`）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

  ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

* 之前提及到的git add和git commit的具体操作过程

  第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区

  第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

  可以理解为`需要提交的文件修改通过git add通通放到暂存区，然后，通过git commit一次性提交暂存区的所有修改`

* 继续对readme.txt进行修改，并创建LICENSE文件，然后用git status查看此时**工作区**的状态

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt
  
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          LICENSE
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

  

  上述告诉我们，在工作区中readme.txt已经被修改，并且多了一个还未被添加过的LICENSE文件

* 用两次git add将readme.txt和LICENSE都添加

  `$ git add readme.txt`

  `$ git add LICENSE`

* 用git status再次查看**工作区**的状态

  ~~~bash
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          new file:   LICENSE
          modified:   readme.txt
  ~~~

  此时，工作区所提交的修改都放到了暂存区（Stage），有一个新的文件LICENSE和一个修改过的文件readme.txt，暂存区的状态如下：

  ![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

* 执行git commit将暂存区的修改一次性提交到master分支

  ~~~bash
  $ git commit -m 'understand how stage works'
  [master b3c44a0] understand how stage works
   2 files changed, 3 insertions(+), 1 deletion(-)
   create mode 100644 LICENSE
  ~~~

* 提交后，工作区就是干净的，用git status查看状态

  ~~~bash
  $ git status
  On branch master
nothing to commit, working tree clean
  ~~~

  此时工作区和暂存区状态如下：
  
  ![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)

