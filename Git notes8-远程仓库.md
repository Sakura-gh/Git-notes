# Git notes 远程仓库

* 找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交；也可以使用github进行Git仓库托管服务

### 实现ssh加密设置连接

* 首先创建SSH KEY

  ~~~bash
  $ ssh-keygen -t rsa -C "youremail@example.com"
  ~~~

  一路回车默认即可

* 在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人

* 登陆GitHub，打开“Account settings”，“SSH Keys”页面：

  然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

  ![github-addkey-1](https://www.liaoxuefeng.com/files/attachments/919021379029408/0)

* 当然，GitHub允许添加多个Key，假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了

### 实现本地仓库推送到远程仓库

* 在github上创建一个repository，并输入相应的文件名，记得不要勾选readme选项，现在已经在github上有了一个Git的托管

* 现在要把本地的仓库与github上的远程仓库关联起来

  ~~~bash
  $ git remote add origin git@github.com:BarasKing/learngit.git
  ~~~

  其中`BarasKing`是我的github账户名，`learngit`是刚刚创建的repository名称，`origin`是远程仓库的名称，当然这是默认叫法，也可以改成别的

* 关联完成后，就可以把本地仓库的内容推送到远程仓库上，第一次推送时，要用`git push -u origin master`，这里的`-u`参数不仅会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

  ~~~bash
  $ git push -u origin master
  Enumerating objects: 23, done.
  Counting objects: 100% (23/23), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (18/18), done.
  Writing objects: 100% (23/23), 1.82 KiB | 311.00 KiB/s, done.
  Total 23 (delta 6), reused 0 (delta 0)
  remote: Resolving deltas: 100% (6/6), done.
  To github.com:BarasKing/learngit.git
   * [new branch]      master -> master
  Branch 'master' set up to track remote branch 'master' from 'origin'.
  ~~~

  此时刷新github页面，即可看到上面的页面已经更新了

* 此后，只要本地仓库上有新修改的内容，即可通过`git push origin master`来推送到远程仓库

  比如我在原来的readme.txt增加了`---`

  ~~~bash
  $ git add readme.txt
  $ git commit -m 'add ---'
  [master 2300c5c] add ---
   1 file changed, 2 insertions(+), 1 deletion(-)
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)
  
  nothing to commit, working tree clean
  $ git push origin master
  Enumerating objects: 5, done.
  Counting objects: 100% (5/5), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (3/3), done.
  Writing objects: 100% (3/3), 299 bytes | 299.00 KiB/s, done.
  Total 3 (delta 1), reused 0 (delta 0)
  remote: Resolving deltas: 100% (1/1), completed with 1 local object.
  To github.com:BarasKing/learngit.git
     4cb786f..2300c5c  master -> master
  ~~~

* 从远程库克隆到本地库则使用`git clone`命令

  ~~~bash
  $ git clone git@github.com:KarasKing/gitlearn.git
  ~~~

### 小结

* 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`
* 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容
* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改

