---
title: Git学习笔记1
date: 2020-01-15 22:46:24
tags: Git
---

## Git安装 ##

### 在Linux上安装Git ###

首先，您可以试着输入`git`，看看系统有没有安装Git:

```java
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

如果您碰巧用Debian或Ubuntu Linux，通过一条`sudo apt-get install git`就可以直接完成

Git的安装。老一点的eDebian或者Ubuntu Linux，要把命令改为`sudo apt-get install git-core`
### 在Mac OS X上安装Git ###

1. 安装homebrew，然后通过homebrew安装Git，具体方法参考homebrew的文档:http://
   brew.sh/
2. 直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜
   单"Xcode"->"Preferences",在弹出窗口找到"Downloads"，选择"Command Line Tools",
   点"Install"就可以完成安装。

### 在Windows上安装Git ###

可以直接从Git官网直接下载安装程序。默认下一步即可。

在开始菜单里找到"Git"->"Git Bash"，蹦出一个类似命令行的窗口。就说明Git安装成功！

**安装成功后，还需要最后一步设置:**
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
由于Git是分布式版本控制系统，所以，每个机器都必须自报家门:你的名字和email地址。

> `--global`参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址.

## 创建版本库 ##

> 什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单的理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都可以跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以"还原".

1. 首先，选择一个合适的地方，创建一个空目录:
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
`pwd`命令用于显示当前目录，从上可以看出，这个仓库位于`/Users/michael/learngit`

> 如果您使用的是Windows系统，为了避免遇到各种各样莫名其妙的问题，请确保目录名(包括父目录)不包含中文。

2. 通过`git init`命令把这个目录变成Git可以管理的仓库。

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/. git/
```

瞬间Git就把仓库创建好了，而且告诉你是一个空的仓库(empty Git repository),自动生成的一个.git目录千万不要乱动！.git目录默认是隐藏的，使用'ls -ah'命令就可以看到.

## 提交 ##

1. 编写一个`readme.txt`文件:
```
Git is a version control system
Git is free softwware
```
> 注:一定要放到创建的`learngit`目录下(子目录也可以)，因为这是一个Git仓库，放到其他地方Git是不会找到这个恩建的。而把一个文件放到Git仓库只需要两步。

* 使用命令`git add`告诉Git，要把文件添加到仓库:

`git add readme.txt`

执行上面的命令，不会有任何显示。这就对了，Unix的哲学:“没有消息就是好消息”，这说明添加成功。

* 使用命令`git commit`告诉Git，把文件提交到仓库:

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入`-m "xxx"`行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

> 为什么将文件提交到仓库需要两步呢，因为`git add <file>`的命令可以多次使用，最后再`git commit`一次性提交，非常方便。

**当命令出现end结束后，无法退出时，可以直接按q结束**

## 版本回退 ##

在Git中，我们可以使用`git log`命令来查看我们提交过的具体版本:

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```
`git log`命令显示从最近到最远的提交日志。如果嫌输出信息听太多，可以加上`--pretty=oneline`

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

> 以上输出信息中`1094abd...`的东西是`commid id`（版本号）.在Git中，用'HEAD'表示当前版本，也就是最新的提交`1094adb...`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`,也可以写成`HEAD~100`，表示第100个版本。

如果想要回退版本，那么可以使用`git reset --hard HEAD^`

查看版本是否被还原 :

```
$ cat readme.txt
aaaaaaaa
bbbbbbbb
```

再用`git log`命令查看版本库的状态，可以看到最新的版本已经看不到了。

如果你想回退的版本已经没有了，那么只要命令行中还存在`1094abd`之类的版本号，那么依旧可以回退。

```
$git reset --hard 1094a
```
如果你的命令行重新打开了，却还想要回退到之前的某个版本，那么使用命令`$ git reflog`用来记录你的每一次命令。

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

##  工作区和暂存区 ##

Git中存在着暂存区的概念。

**工作区**指的就是你在电脑里能看到的目录，比如创建的包含`.git`的文件夹就是一个工作区。

**版本库**工作区中有一个隐藏的目录`.git`，他不算是工作区，而是Git的版本库。Git的版本库中存放了很多东西，其中，最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的 第一个分支`master`，以及指向`master`的一个指针`HEAD`
![示意图](Git学习笔记/工作区暂缓区.jpg)

可以用命令试着去比较一下工作区，暂缓区和版本库之间的区别。

**git diff的命令只会比较两区域都存在的文件,如果工作区存在a文件,暂存区不存在a文件,其他都相同,那么git diff也不会打印出任何东西.这是可以使用git status命令比较查看.**

`git status` ---查看状态。

`git diff` ----查看工作区和暂存区差异

`git diff --cached` ----查看暂存区和仓库的差异

`git diff HEAD` ----查看工作区和仓库的差异

`git add`的反向命令`git checkout --file`，撤销工作区修改，如果修改后还没有放到暂存区，那么就是仓库的最新版本转至工作区。如果已经添加到暂存区，又做了修改，那么撤销修改就是回到添加到暂存区后的状态。

`git commit`的反向命令`git reset HEAD`,就是把仓库最新版本转移至暂存区。	

> `git checkout --file`命令中的`--`很重要，没有`--`，就变成了'切换到另一分支'的命令。

当你不小心把工作区的内容添加到了暂缓区，那么就可以使用`git reset HEAD<file>`命令把暂存区的修改回退到工作区。当我们使用`HEAD`时，表示最新的版本。

`git reset`命令个既可以回退版本。也可以把暂存区的修改回退到工作区。

## 删除文件 ##

一般情况下，通常直接删除没用的文件，或者使用`rm`命令删了:

`$ rm test.txt`

这种时候，工作区和版本库就不一致了。可以`git status`命令 查看状态。

现在，如果你想要从版本库中删除该文件，那么就是用命令`git rm`删掉，然后`git commit`

还有一种情况是删错了，那么直接`$ git checkout --test.txt`



## 总结 ##
`git checkout file`和`git reset HEAD file`

1. 当你只是单纯的修改了工作区的内容，当发现修改的内容错误时，就可以使用`git checkout file`回退工作区的内容。

> 当你的暂存区有对应文件的话，那么回退得版本是暂存区的版本，当暂存区已经提交或没有存在的话，那么回退的版本就是版本库最近一次的版本。(据我自己试验可得，此命令就是把暂存区的内容回退到工作区.)

2. 当你已经把工作区的内容添加到了暂存区，那么你想撤销掉暂存区的内容，就可以使用`git reset HEAD file`命令，把暂存区的内容撤回到版本库最近一次的版本，然后再使用`git checkout file`命令把工作区的版本回退到暂存区的最近一次版本。

> `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

3. 当你把错误的文件提交到版本库时，参考上面的回退版本。


**本节命令**

- git设置用户名邮箱:  

	`$ git config --flobal user.name "Your Name"`

	`$ git config --global user.email "email@ex.com"`

- 本地创建空目录:

	`$ mkdir emptyPro`

- 进入创建的目录

	`$ cd emptyPro`

- 查看当前所在目录

	`$ pwd`

- 初始化空目录使之成为可以被Git管理的仓库（注意是要在这个目录下）

	`$ git init`

- 把编写的文件添加到暂存区

	`$ git add one.txt`

	`$ git add two.txt`

- 把添加到暂存区的文件全部提交到仓库

	`$ git commit -m "tips"`

- 查看提交过的版本

	`$ git log`

- 查看格式化后的版本

	`$ git log --pretty=oneline`

- 工作区的内容回退一个版本(hard表示HEAD指向目标版本，且工作区和暂存区的内容都要回退到目标版本。)

	`$ git reset --hard HEAD^`


- 回退两个版本

	`$ git reset --hard HEAD^`

- 回退到第n个版本

	`$ git reset --hard HEAD~n`

- 查看文件内容

	`$ cat one.txt`

- 查看所有变更记录的版本号

	`$ git reglog`

- 查看状态

	`$ git status`

- 查看工作区和暂存区差异

	`$ git diff`

- 查看暂存区和仓库差异

	`$ git diff --cached`

- 查看工作区和仓库差异

	`$ git diff HEAD`

- 删除工作区文件

	`$ rm one.txt`

- 删除暂存区文件夹

	`git rm -r testPro`

- 删除暂存区文件

	`$ git rm one.txt`

- 撤销工作区的修改

	`$ git checkout file`

- 如果有多个分支的话撤销工作区的修改（表示撤销当前工作分支的工作区的修改）

	`$ git checkout HEAD file`

- 撤销暂存区的修改

	`$ git reset HEAD file`

- 查看暂存区内容

	`$ git ls-files --stage`

- 清空(删除)暂存区

	`$ rm .git/index`

