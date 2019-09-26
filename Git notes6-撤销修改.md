# Git notes 撤销修改

* 如果在readme.txt中不小心添加了不该添加的东西，例如在末尾加了一句

  ```
  Git is a distributed version control system.
  Git is free software distributed under the GPL.
  Git has a mutable index called stage.
  Git tracks changes of files.
  My stupid boss still prefers SVN
  ```

* 如果**还没有使用add加入暂存区或commit提交到本地仓库**，可以手动在txt中删除最后一行，以回到上一个版本，当然此时我们先用git status查看一下目前的信息

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt`
  ~~~

  其中`use "git restore <file>..." to discard changes in working directory`告诉我们**git restore filename**即可把`readme.txt`文件在**工作区**（并非暂存区或版本库）的修改全部撤销

  ~~~bash
  $ git restore readme.txt
  ~~~

  这里有两种情况：

  一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

  一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

  总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态

* 此时再查看readme.txt的内容

  ~~~bash
  $ cat readme.txt
  Git is a distributed version control system
  Git is free software distributed under the GPL
  Git has a mutable index called stage
  Git tracks changes of files
  ~~~

  最后一句`My stupid boss still prefers SVN`已经消失
  
* 除了**git restore filename**之外，还有**git checkout -- filename**可以撤销工作区的操作

  ~~~bash
  $ git checkout -- readme.txt
  ~~~

  

* 如果之前一不小心把这段话传到了暂存区，ok，那就需要进行两步走，**第一步是把内容从暂存区回退到工作区，第二步是撤销工作区的修改**

* 在修改完txt后，将其加入到暂存区

  ~~~bash
  $ git add readme.txt
  ~~~

* 按照惯例，我们用git status查看工作区的状态，顺便看一下bash对如何回退的提示

  ~~~bash
  $ git status
  On branch master
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          modified:   readme.txt
  ~~~

* 上文提示`use "git restore --staged <file>..." to unstage`告诉了我们从暂存区回退的方式

  ~~~bash
  $ git restore --staged readme.txt
  ~~~

  **此时已经完成了第一步即把内容从暂存区回退到工作区**

* 使用`git status`查看工作区状态来验证这一情况

  ~~~bash
  $ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
          modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ~~~

  `Changes not staged for commit`表示目前暂存区里是干净的，但是工作区是有修改的

* 随后便用之前的方法`git restore`把工作区里的修改撤销

  ~~~bash
  $ git restore readme.txt
  ~~~

  **此时已经完成了第二步即撤销工作区的修改**

* 最后再用`git status`来查看工作区的状态

  ~~~bash
  $ git status
  On branch master
  nothing to commit, working tree clean
  ~~~

  ok，工作区的修改也已经被撤销完成

* **如果内容已经从暂存区被推送到本地仓库，则需要用`git reset --hard file`来进行版本回退，前提是还没有推送到远程仓库**

