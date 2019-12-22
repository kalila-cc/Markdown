[TOC]

---

# Git操作

```
$ git config --global user.name <name>
$ git config --global user.email <email>
// 因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：名字和Email地址；--global 参数表示你这台机器上所有的Git仓库都会使用这个配置
$ git config --global alias <new> <old>
// 给某些字符串取别名，若有多个单词，用单引号括起来
$ git init
// 把当前目录变成Git可以管理的仓库
$ git add [-A]|<file>
// 把文件添加到版本库，被修改的文件都需要添加到暂存区才能正常 commit，-u 参数则添加多个文件
$ git commit -m <message>
// 提交变动的文件目录，-m 后面的参数是本次提交的说明，可以输入任意内容
$ git status
// 返回仓库当前的状态
$ git diff
// 返回文件变动的信息
$ git diff HEAD -- <file>
// 查看文件在工作区和版本库里面最新版本的区别
$ git reset HEAD <file>
// 把暂存区的修改撤销掉，常配合 git checkout -- <file> 使用
$ git reset --hard HEAD^[^...] | <commit-id>
// 把当前版本回退到上几个版本，HEAD表示当前版本，若使用 <commit-id> 可以不用写全
$ git log [--graph] [--pretty=oneline] [--abbrev-commit]
// 查看提交历史
$ git reflog
// 查看命令历史，即使命令行窗口已经被关闭过
$ git checkout -- <file>
// 先从缓存区中拉取版本还原，如果没有再到版本库中拉取还原
$ git checkout [-b] <branch> [origin/<branch>]
// 切换分支， -b 参数则表示创建分支并切换，origin/<branch> 则表示在本地创建和远程分支对应的分支
$ git remote [-v]
// 查看远程库的信息，-v 参数则为显示更详细的信息，若没有push权限则无法看到push地址
$ git remote add <origin> git@github.com:<server>/<repo>.git
// 关联远程库
$ git push [-u] <origin> <branch>|<tag>|--tags
// 把本地库的内容推送到远程库，首次推送需要 -u ，，若推送的是master，则此时Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
$ git push origin :refs/tags/<tag>
// 删除远程标签
$ git clone git@github.com:<server>/<repo>.git
// 克隆一个远程库到本地，再通过 cd <repo> 进入该库
$ git branch [-d|-D] [<branch>]
// 有参数则创建/删除(-d)/强制删除(-D)分支，无参数则查看当前分支
$ git branch --set-upstream-to=origin/<branch> <branch>
$ git branch --set-upstream <branch> origin/<branch>
// 指定本地<branch>分支与远程origin/<branch>分支的链接
$ git pull
// 将已绑定链接的远程库同步至本地库
$ git merge [--no-ff] [-m <message>] <branch>
// 合并某分支到当前分支，可同时指定 commit 信息，合并默认策略为 fast forward ，看不出曾经合并过；使用 no-ff后合并策略变为 recursive ，能够看出曾经合并过
$ git stash [list|apply <commit-id>|pop]
// 保存工作区状态，使用 list 参数则列出所有存档，使用 apply 参数则恢复存档，使用 pop 参数则恢复并删除存档
$ git cherry-pick <commit-id>
// 复制一个特定的提交到当前分支
$ git rebase
// 把本地未push的分叉提交历史整理成直线，使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比
$ git tag [-d] [[-a] <tag> [<commit-id>]] [-m <message>]
// 为指定分支添加标签，可指定<commit-id>，可添加标签信息，，可删除标签，无参数则表示查看所有标签
$ git show <tag>
// 查看标签信息
$ git check-ignore -v <file>
// 	查看文件忽略信息，忽略规则将由'.gitignore'文件决定，文件格式见https://github.com/github/gitignore
```

# 路径操作

```
$ cd <path>
// 移动当前目录
$ pwd
// 显示当前目录
$ ls -ah
// 显示当前目录所有文件，包括隐藏文件
```

# 文件操作

```
$ mkdir <dir>
// 创建文件夹
$ touch <file>
// 当 git add <file> 无法添加 .gitignore 内的文件时可以用这个
$ ssh-keygen -t rsa -C <email>
// 在 users 用户主目录下的 .ssh 文件夹中有 id_rsa / id_rsa.pub 两个文件，分别是密钥和公钥
```

