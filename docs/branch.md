# Git基础知识教程整理（Git分支管理）

## 分支的创建、合并与删除

### 创建分支与切换分支

> $ git branch develop

> $ git checkout develop

或者

> $ git checkout -b develop

git checkout命令加上-b参数表示创建并切换。git branch或者（git branch -a)后面不跟分支名时指列出所有分支，当前分支前面加*。

> $ git branch

### 合并分支

git merge命令用于合并指定分支到当前分支，如果当前分支是master分支，git merge develop指将develop分支合并到master分支。

> $ git merge develop

### 删除分支

删除本地develop分支，不能在当前分支执行删除当前分支的操作。

> $ git branch -d develop

## 解决冲突

冲突可以说是两个分支的冲突，产生的原因是两个已经提交的分支的相同文件相同位置的的不同操作进行了合并

多人协作开发的时候，如果出现了你没有改过的文件跟你冲突了，一定要去找到当事者，说清楚是如何冲突，然后协商解决，修文件，确保没问题后在重新add、commit、push。

一般代码编辑器都集成了git，如WebStrom、VsCode，可以很直观的查看冲突代码，并进行代码合并。

## Rebase操作

### 合并多个commit为一个完整commit

> $  git rebase -i  [startpoint]  [endpoint]

其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint]  [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。如果不指定分支默认操作当前分支

### 将当前分支的一段commit粘贴到另一个分支上

> $ git rebase   [startpoint]   [endpoint]  --onto  [branchName]

### rebase的优点和缺点
优点 
- rebase最大的好处是你的项目历史会非常整洁 
- rebase 导致最后的项目历史呈现出完美的线性——你可以从项目终点到起点浏览而不需要任何的 fork。这让你更容易使用 git log、git bisect 和 gitk 来查看项目历史

缺点 
- 安全性，如果你违反了 rebase 黄金法则（绝不要在公共的分支上使用它），重写项目历史可能会给你的协作工作流带来灾难性的影响 
- 可跟踪性，rebase 不会有合并提交中附带的信息——你看不到 feature 分支中并入了上游的哪些更改

### 修复冲突

git rebase --abort会回到rebase操作之前的状态，之前的提交的不会丢弃。

> $ git rebase --abort

git rebase --skip则会将引起冲突的commits丢弃掉。

> $ git rebase --skip

git rebase --continue用于修复冲突，提示开发者，一步一步地有没有解决冲突，fix conflicts and then run "git rebase --continue"。

> $ git rebase --continue

### 远程协作

本地仓库和远程仓库，Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

查看远程库（git remote)

> $ git remote

查看远程库详细信息

> $ git remote -v

往远程仓库推送代码，须选择本地分支，下面指往develop分支推送代码

> $ git remote origin develop

只有需要协同开发的才需要往远程仓库推送代码

+ master分支是主分支，因此要时刻与远程同步；
+ dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
+ bug分支只用于在本地修复bug，不用推送到远程；
+ feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 标签管理

创建标签 (git tag \<tagname\>)

> $ git tag v1.0

查看标签（git tag)

> $ git tag

默认标签是打在当前分支最新提交的commit上的，如果要打在历史的commit上，找到历史提交的commit id（git tag v0.9 \<commit id\>）

> git tag v0.9 f52c633

查看标签信息（git show \<tagname\>）

> $ git show v0.9

创建带有说明的标签，用-a指定标签名，-m指定说明文字

> $ git tag -a v0.1 -m "version 0.1 released" 1094adb
