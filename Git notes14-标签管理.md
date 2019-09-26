# Git notes 标签管理

* 在Git中打标签非常简单，首先，切换到需要打标签的分支上：

  ```bash
  $ git branch
  * dev
    master
  $ git checkout master
  Switched to branch 'master'
  ```

  然后，敲命令`git tag <name>`就可以打一个新标签：

  ```bash
  $ git tag v1.0
  ```

  可以用命令`git tag`查看所有标签：

  ```bash
  $ git tag
  v1.0
  ```

* 可以找到历史提交的commit id，然后打上相应的标签tag：

  ```bash
  $ git log --pretty=oneline --abbrev-commit
  12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
  4c805e2 fix bug 101
  e1e9c68 merge with no-ff
  f52c633 add merge
  cf810e4 conflict fixed
  5dc6824 & simple
  14096d0 AND simple
  b17d20e branch test
  d46f35e remove test.txt
  b84166e add test.txt
  519219b git tracks changes
  e43a48b understand how stage works
  1094adb append GPL
  e475afc add distributed
  eaadf4e wrote a readme file
  ```

  比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

  ```bash
  $ git tag v0.9 f52c633
  ```

  再用命令`git tag`查看标签：

  ```bash
  $ git tag
  v0.9
  v1.0
  ```

* 可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

  ```bash
  $ git tag -a v0.1 -m "version 0.1 released" 1094adb
  ```

  用命令`git show <tagname>`可以看到说明文字：

  ```bash
  $ git show v0.1
  tag v0.1
  Tagger: Michael Liao <askxuefeng@gmail.com>
  Date:   Fri May 18 22:48:43 2018 +0800
  
  version 0.1 released
  
  commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
  Author: Michael Liao <askxuefeng@gmail.com>
  Date:   Fri May 18 21:06:15 2018 +0800
  
      append GPL
  
  diff --git a/readme.txt b/readme.txt
  ...
  ```

   注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签

* 如果标签打错了，也可以删除：

  ```bash
  $ git tag -d v0.1
  Deleted tag 'v0.1' (was f15b0dd)
  ```

* 如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

  ```bash
  $ git push origin v1.0
  Total 0 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
   * [new tag]         v1.0 -> v1.0
  ```

  或者，一次性推送全部尚未推送到远程的本地标签：

  ```bash
  $ git push origin --tags
  Total 0 (delta 0), reused 0 (delta 0)
  To github.com:michaelliao/learngit.git
   * [new tag]         v0.9 -> v0.9
  ```

* 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

  ```bash
  $ git tag -d v0.9
  Deleted tag 'v0.9' (was f52c633)
  ```

  然后，从远程删除。删除命令也是push，但是格式如下：

  ```bash
  $ git push origin :refs/tags/v0.9
  To github.com:michaelliao/learngit.git
   - [deleted]         v0.9
  ```

* ### 小结

  - 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
  - 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
  - 命令`git tag`可以查看所有标签；
  - 命令`git push origin <tagname>`可以推送一个本地标签；
  - 命令`git push origin --tags`可以推送全部未推送过的本地标签；
  - 命令`git tag -d <tagname>`可以删除一个本地标签；
  - 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签