## Git 命令学习

> 使用 Git Bash 进入编辑文本模式。
>
> 使用vi+文件名，新建一个文件并进入编辑状态。（vi有两种模式，编辑模式和命令模式）
>
> 在编辑模式下（使用 i 命令进去），使用esc就可以切换到命令模式。

**下面的这几个常用的命令，都是在命令模式下，输入:+命令使用**

- q退出编辑，如果文本内容被修改过，则会报错。
- q!强制退出编辑，如果文本内容被修改过，会丢弃此次的修改。
- x退出编辑并保存。

> git add <file>将文件放到暂存区
>
> git commit -m <message> 将文件添加到仓库 （可选参数  -m 后面输入的是本次提交的说明）（commit 可以一次性提交很多文件）
>
> git diff 用来查看文件被修改的内容

### 版本回退

> git log 用来查看历史记录（提交历史）显示从最近到最远的提交日志 （可选参数 --pretty=oneline）

**在Git中 用 HEAD 表示当前版本 上一个版本就是HEAD^ 上上一个版本就是HEAD^^ 或者是HEAD~number**

> git reset --hard commit_id    来进行版本回退
>
> git reflog 用来记录你的每一次命令（命令历史）

### 工作区和暂存区

工作区：就是你在电脑里能看到的目录

版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存放了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD

> 做文件做了修改如何进行撤销
>
> (1) 文件已经被修改但是没有被 add 到暂存区
>
> git checkout -- file 将文件在工作区的修改全部撤销掉
>
> (2)修改的文件已经被add 到暂存区，但还没有被commit 到仓库里
>
> git reset HEAD file 可以把暂存区的修改撤销掉，重新放回到工作区
>
> (3)修改的文件已经被commit 到仓库里，但是还没有push 到远程仓库
>
> git reset --hard commit_id   来进行版本回退

### 文件删除

rm file 删除文件

> (1)新文件已经被添加并提交的仓库，但还是要删除
>
> git rm file
>
> git commit 
>
> (2)文件是被误删的，但是仓库中存有文件

### 远程仓库

> git remote add alias address 添加远程仓库
>
> git push -u origin master   第一次推送时加上可选参数-u （git不但会把本地的master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来）

### 分支管理

> git checkout -b name  表示创建并切换 （相当于以下两条命令）
>
> git branch name
>
> git checkout name 
>
> git branch  查看当前分支
>
> git merge name   用于合并指定分支到当前分支
>
> git branch -d name 删除指定分支 

### 解决冲突

git log --graph 查看分支合并图  

**如果用 git log 查看历史提交命令 过长时 使用q 来进行退出操作**

> master分支应该是非常稳定的，也就是仅用来发布新版本，平时不用。
>
> dev分支应该是用来干活的，即dev分支是不稳定的，到某个时候，再把dev分支合并到master上即可。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。 

### BUG分支

> git stash 把当前工作内容储存在某地，等以后恢复现场后继续工作
>
> git stash list 命令查看被储存的工作区

恢复被储存的工作区有两个办法

* git stash apply 恢复   但是恢复后，stash内容并不删除，你需要git stash drop来删除
* git stash pop 恢复的同时把stash内容也删除了

git branch -D name 强行删除（如果要丢弃一个没有合并过的分支）

**当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应了起来，并且远程仓库默认名称是origin**

### 多人协作

> git remote -v 查看远程仓库信息
>
> git push origin branch-name 推送分支（推送时要指定本地分支，这样git会把该分支推送到远程仓库对应的远程分支上）
>
> 从远程仓库clone时，默认情况下，只能看到本地的master分支
>
> 要想在本地分支上开发，就必须创建远程origin的dev分支到本地
>
> git checkout -b dev origin/dev  在本地创建和远程分支对应的分支 




> 因此，多人协作的工作模式通常是这样：
>
> 1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
> 2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
> 3. 如果合并有冲突，则解决冲突，并在本地提交；
> 4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
>
> 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。
>
> 这就是多人协作的工作模式，一旦熟悉了，就非常简单。



