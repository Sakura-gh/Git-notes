# Git notes 自定义

可以把把`lg`配置成：

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

来看看`git lg`的效果：（查看分支的合并情况）

![git-lg](https://www.liaoxuefeng.com/files/attachments/919059728302912/0)