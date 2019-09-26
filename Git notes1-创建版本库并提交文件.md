# Git notes-创建版本库并提交文件

* 创建版本库

  ~~~bash
  $ mkdir learngit
  $ cd learngit 
  $ pwd
  ~~~

  设置Git管理（当前文件夹目录下）

  ~~~bash
  $ git init
  ~~~

* 加入TXT文件

  readme.txt

  ​	Git is a version control system

  ​	Git is free software

* 添加到暂存区

  ~~~bash
  $ git add readme.txt
  ~~~

* 从暂存区提交到本地仓库

  ~~~bash
$ git commit -m 'wrote a readme file'
  ~~~
  
  注：-m 后跟着的字符串为提交说明

