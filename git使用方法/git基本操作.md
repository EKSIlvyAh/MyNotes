# Git基本操作

## git工作流

首先git会将项目分为不同的分支

每个分支都分为4个区域
* 工作区
* 暂存区
* 本地仓库
* 远程仓库

----

gti主要通过branch，add，commit，push等命令来操作不同的区块

命令|对应区块
:-:|:-:
barch|分支
add|暂存区
commit|本地仓库
push|远程程仓库

不同分支内的文件互不影响（除非进行合并）

## 每个分支的主要工作流程如下

使用add命令将修改的文件添加到暂存区

使用commit命令将修改的文件提交的本地仓库保存

使用push命令将修改后的项目提交的远程仓库保存

***
下面是具体的操作流程
====
***

## 创建版本控制仓库

首先使用git初始化文件夹

设置全局用户名和邮箱（用于记录是谁修改了文件）

初始化之后会在该文件夹创建 **.git文件夹**

**.git文件夹** 内包含git进行版本控制的所需文件

同时本地仓库也位于 **.git文件夹** 内

初始化后会默认处于主分支里面，名字默认为（master）
```
cd 文件夹路径

git config --global user.name "填写名字
git config --global user.email "邮箱"

git init
```

## 查看仓库状态

**该命令展示的信息有**
* 处于的分支
* 改变并且可以提交的文件
* 改变了但未追踪的文件
* 未追踪的文件

是个非常常用的命令

```
git status
```

## 将工作区文件提交到暂存区

在使用git时，文件会有基本的4种状态

状态|含义
:-:|:-|
Untracked(未追踪)|未add或者未添加到.gitignore文件的文件
tracked(已追踪)|通过add添加到暂存区的文件
Unstage|已追踪但是有变动未add的文件
stage|已追踪无变更的文件

使用 **add** 命令可以将文件添加到暂存区来确定跟踪状态

如果是已追踪并且修改的文件，也需要进过此步骤才可往下进行

```
git add "文件名（路径）", ... （可以添加多个文件）
```

## 撤销对文件的修改

如果不满意对文件的修改

可以使用restore命令把**处于工作区的文件**恢复到最近一次add的内容

```
git restore "文件名（路径）", ... （可以添加多个文件）
```

## 撤销add

同样的add也是可以撤销的

撤销之后状态

```
git rm --cached (文件名...)
```

## 将暂存区的文件提交到本地仓库

使用commit命令即可

如果不指定文件，默认提交暂存区所有内容

```
git commit "文件名（路径）", ... （可以添加多个文件）
```

这时，会进入默认的编辑器（vim，VSCode等，具体看个人是否配置）

这个打开的文件可以编辑关于的commit的说明

关闭文件后即编辑成功

如果提交时只需写一点简短的说明，不想打开编辑器，可以使用以下命令

```
git commit (文件名...) -m "填写说明"
```

## 查看提交日志

```
git log

# 输入之后，会看到如下类似的信息

commit 4f4efff220b6313eb35f1589bf92224685cb204d (HEAD -> main)
Author: EKSIlvyAh <1926139610@qq.com>
Date:   Thu Jan 12 14:54:04 2023 +0800

    第一次提交
```
**其中**

**4f4efff220b6313eb35f1589bf92224685cb204d** 

这段哈希字符串代表不同的提交

**(HEAD -> main)** 

所在的位置代表代表当前所在版本位置

**main** 

代表所处分支

## 快速提交文件到本地仓库

```
git commit (文件名...) -a -m "提交描述"
```
这样相当与一次性执行了 add 和 commit

下面这种写法也是可以的

```
git commit -am "提交描述"
```

## 忽略不需要提交的文件

在工作区创建 **.gitignore** 文件

然后在里面写上不需要提交的**文件**或者**文件夹**即可

当然也可以忽略 **.gitignore** 文件本身

## 创建新分支

```
git branch "分支名"
```

这样会在主分支上创建一个新分支

新分支的所有文件都是从主分支复制而来

会展示所有创建的分支并标明当前分支

## 切换分支

```
chekout "分支名"
```

## 快速创建分支并切换分支

```
git checkout -b "分支名"
```

## 删除分支

```
git branch -d "分支名"
```

使用该命令有个前提条件 **在分支与主分支合并后才可删除**

```
git brach -D "分支名"
```

使用该命令可强制删除，无论是否合并

## 合并分支

```
git merge "分支名"
```

把**别的分支**合并**到当前分支**上来

<font color="yellow">**但是**</font>

要想合并，是需要满足一定条件的

如果不满足，就会引发冲突(conflict)

例如

两个分支都有同一个文件，但是该文件的某一行内容在两个分支上不一样

就会引发冲突

## 查看所有分支

```
git brach
```

## 关于分支

每个分支都有项目文件的副本（.gitignore内包含的文件不算，下面也一样）

所以，每次切换分支都会更换工作区的文件

即使某个分支的文件被删除了，也不会影响其他分支

## 创建新仓库到github

主要使用push命令，同时在github注册有账号

下面是在命令行创建一个空仓库的示例流程

```
echo "# 项目名称" >> README.md
git add README.md
git commit -m "第一次提交"
git branch -M main
git remote add origin https://github/EKSIlvyAh/TestGit2.git
git push -u origin main
```

## 上传一个已存在的仓库

```
git remote add origin https://github/EKSIlvyAh/TestGit2.git
git branch -M main
git push -u origin main
```

## 从github克隆仓库

git clone **[HTTPS/SSH/GitHub CLI]**

连接方式|格式
:-:|:-:
HTTPS|https://github.com/你的用户名/项目名.git
SSH|git@github.com:你的用户名/项目名.git

例如
```
git clone https://github.com/EKSlivyAh/the-git-test.git

git clone git@github.com:EKSlivyAh/the-git-test.git
```

## 查看远程链接

```
git remote -v
```
会显示远程仓库的别名及地址

## 从远程仓库拉取新分支

git fetch --prune
拉取clone的远程仓库并强行替换本地的库

----

pull origin main
就是拉取了,并同时刷新本地的main分支

