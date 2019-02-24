# Git基础知识教程整理（附git常用命令表）

## Git简介
Git是目前世界上最先进的分布式版本控制系统（没有之一）。
Linux之父Linux用C语言写了Git分布式版本控制系统。

## 分布式版本控制系统与集中式版本控制系统的区别

|区别 |分布式  |集中式  |
|:----------- |:------|:------|
|中央服务器 |有，版本库集中存放在中央服务器，工作时需从中央服务器获取最新版本代码，工作完成后在将代码推送到中央服务器。中央服务器出了问题，开发者几乎无法工作 |无，开发人员本地都有本地存储库（Local Repository）|
|联网|必须，必须要联网才能工作，而且对网络的依赖性较强，网络较差或者文件较大，文件提交的速度会受很大的限制|不是必要的，在没有网络的情况下也可以执行commit、查看版本提交记录、以及分支操作，在有网络的情况下执行 push 将代码从本地仓库推送到远程仓库（Remote Repository）。|
|存储格式|原始文件，体积大|元数据，体积小|
|分支操作|创建新的分支则所有的人都会拥有和你一样的分支|分支操作不会影响其他开发人员|
|提交方式|直接提交到中央版本库|先commit到本地仓库，再push到远程仓库|
|优点|方便管理，逻辑明确，上手快；更能保证代码的安全性；代码一致性非常高；有良好的目录级权限控制系统|速度快、灵活，分支之间可以任意切换；单机合并分支，开发者之间容易解决冲突；可离线工作，不影响本地代码编写；公共服务器压力小，数据传输量少|
|缺点|服务器性能要求高，数据体量大；必须联网；不适合开源；分支管理不灵活|不符合常规思维，学习周期相对较长；代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息。|

## 创建版本库

1. 创建一个项目目录（mkdir Git）
2. 进入到这个目录（cd Git）
3. 初始化版本库（git init）

在当前目录下会有.git的目录，它是git进行跟踪和管理版本库，禁止删改此文件(如果没看到可能是您的电脑不显示隐藏文件,在命令行工具运行 ls -ah可查看)。


```
$ mkdir Git
$ cd Git
$ git init
// Initialized empty Git repository in /Users/zhengdahua/Documents/Project/Private/Git/.git/ 初始化空的版本库
```

4. 编写一个README.md文件
5. 将README.md添加到暂存区（git add README.md)
6. 确认提交文件到仓库（git commit -m 'add example file'）

我们当前的项目目录是工作区，git初始化之后会生成一个.git文件，即我们所说的版本库（respository），.git中有一个index文件就是暂存区（stage），git还为我们自动生成了一个分支master以及指向该分支的指针head。想了解更多，请移步[git工作区、暂存区、版本库之间的关系](https://www.cnblogs.com/lianghe01/p/5846525.html)。

```
$ type nul>README.md
$ git add README.md
$ git commit -m 'add example file'
```

## 版本回退

+ 查看版本库的状态（git status）。下面的命令输出告诉我们，document.md被修改过了，但还没有提交。

```
➜  Git git:(master) git status 
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   docs/document.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  Git git:(master) ✗ 
```

+ 比较两次修改的差异（git diff）。“-”后面跟的是删除的内容，“+”后面跟的是新增的内容

```
-### 4. 编写一个README.md文件
-### 5. 将README.md添加到暂存区（git add README.md)
-### 6. 确认提交文件到仓库（git commit -m 'add example file'）
++ 4. 编写一个README.md文件
++ 5. 将README.md添加到暂存区（git add README.md)
```

+ 查看提交日志（git log）。显示从最近到最远的提交日志

```
commit 241f77158c2d59b0b10e482b74a24150a0bebeb4 (HEAD -> master)
Author: kevin <kenvin.zheng@drigle.com>
Date:   Sun Feb 24 22:24:51 2019 +0800

    update document.md

commit f953ccc298a430939e5e64eeedd49cc2db5a3fdb
Author: kevin <kenvin.zheng@drigle.com>
Date:   Sun Feb 24 21:43:08 2019 +0800
```

+ 回退版本（git reset --hard HEAD^）。用HEAD表示当前版本，则HEAD^就是上个版本，HEAD^表示上上个版本，HEAD～100表示上一百个版本，“241f77158c2d59b0b10e482b74a24150a0bebeb4”指的是版本的id，我们要回退到指定版本是只需要id的前几位就行，但最好5位以上。回退指定版本（git reset --hard f953c）。

+ 查看所有分支的版本操作记录（git reflog）。当你用$ git reset --hard HEAD^回退到以前版本时，再想恢复到append GPL，就必须找到append GPL的commit id，通过git reflog就可以append GPL的commit id了。

```
241f771 (HEAD -> master) HEAD@{0}: reset: moving to 241f771
f953ccc HEAD@{1}: reset: moving to HEAD^
241f771 (HEAD -> master) HEAD@{2}: commit: update document.md
f953ccc HEAD@{3}: commit (amend): add first file
bd07a32 HEAD@{4}: commit (amend): add first file
98f7467 HEAD@{5}: commit (initial): add first file
```

## 修改管理
Git 并不跟踪与文件相关的文件名和目录名，而是跟踪的是文件的内容，查看[Git 追踪内容详解](https://blog.csdn.net/sean_8180/article/details/82717204)。因此文件的每一次修改，都需要git add添加到暂存区，然后在commit到版本库。

+ 修改撤销（git checkout -- README.md）。这里有两种情况：一种是README.md自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。

+ 删除文件。工作区直接删除文件，提交到版本库。