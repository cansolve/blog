---
title: Git 使用经验分享
date: 2018-10-7 9:05:40
tags: git
categories: 个人
---

### 版本控制

Git 是分布式版本控制.每一台用户终端都可以充当中央仓库的角色,镜像整个仓库。 Git 主要对提交的全部文件制作一个快照并保存这个快照的索引，所以高效。

### Git开发流程

Git开发过程中牵涉到：工作区、暂存区、仓库（本地仓库和远程仓库）

### Git 本地仓库常用操作

1.  添加与删除暂存区操作
    

```
git add 将文件加入到暂存区域
git add . 添加当前目录的所有文件到暂存区
git add [dir] 添加指定文件或文件夹到暂存区
git add [file1] [file2] ... 添加指定文件到暂存区
git add ./*.java 支持正则表达式
git rm 删除文件.
git rm [file1] [file2] 删除工作区文件，并且将这次删除放入暂存区
git rm --cached [file] 删除暂存索引, 保留文件, 即恢复成untracked状态，并且将这次删除放入暂存区（已跟踪）
```

2.  提交本地库操作
    

```
git commit 将暂存区域的文件提交到版本库
git commit --amend -m [message] 使用一次新的commit，替代上一次提交。如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit -a 则将已跟踪但未暂存的文件和已跟踪且已暂存的文件一起提交
```

3.  本地分支操作
    

```
git checkout [branch] 切换分支
git checkout -b [branch] 创建并且切换分支
git checkout master file_name 放弃当前对文件file_name的修改，用本地库当前分支的文件进行替换
git checkout commit_id file_name 取文件file_name的 在commit_id是的版本。
```

4.  分支合并操作
    

```
git merge [branch] 合并指定分支到当前分支，并且提交版本库，把分支的commit log带到新分支
git merge [branch] --squash 合并指定分支到当前分支，不移动HEAD，不提交。需要进行一次额外的 git commit 来.带来好处不会把分支的 commit 记录带到新分支来。
git merge [master] [branch] 将[master]合并到[branch] 分支
git reset --hard HEAD 可以用此来撤销合并，其实就是回滚合并前的状态
git cherry-pick [commit] 选择一个commit，合并进当前分支，用在部分功能合并
```

### Git 远程仓库常用操作

1.提交到远程库

```
git push origin master
git push origin 当前分支与远程分支之间存在追踪关系（git clone，或手动建立 git branch --set-upstream master origin/next），则本地分支和远程分支都可以省略。
git push 当前分支只有一个追踪分支，那么远程别名都可以省略
```

2.取回远程仓库的变更但并不自动合并当前的工作分支（fetch）

```
git fetch origin master 取回远程 origin 的 master 分支变更该命令经常与 git checkout 搭配使用，用来获取远程仓库的分支
```

3.获取更新 并且合并当前分支（pull）。

```
git pull (= git fetch + git merge)
 git pull 命令相当于 git fetch + git merge, 即从远程仓库中拉取所有你本地仓库中还没有的数据, 并自动合并到当前工作分支.
```

### 开发中常用

一. 获取远程分支，进入分支开发功能,开发完后合并主干，最后提交主版本到远程仓库

```
1 git remote show origin （查看远程分支情况）
    
2 git fetch origin new_branch (获取远程分支的变更到本地仓库)
    
3 git checkout new_branch （切换分支new_branch进行开发）
    
4 开发相关功能文件
    
5 git add file
    
6 git commit
    
7 git checkout master （切换到主干）
    
8 git merge new_branch (把new_branch合并到当前的所在分支)
    
9 git push origin master
    
10 git branch -d new_branch (删除分支)
    
11 git branch (查看当前分支情况，确认删除)
```

二. 文件修改后想还原到上次提交的状态（不保留修改内容）

```
git checkout  (文件未提交到暂存区用法)
git checkout HEAD  （如提交到暂存区执行该命令后，不保存暂存区）
git reset --hard HEAD （如提交到暂存区执行该命令后，不保存暂存区）
```

三. 撤销上一次提交本地仓库的文件

```
1 git log (查看提交版本信息) git reset 前一个SHA值（7位数）（撤销commit，会保留提交前的修改内容）
    
2 git reset --soft HEAD^1 退到commit前，保留stage（暂存区）和提交前修改内容
    
3 git reset --mixed HEAD^1 退到commit前，保留提交前修改内容（默认）
    
4 git reset --hard HEAD^1退到commit前且恢复到未修改时内容（内容退回到上次commit时的状态）
    
5 git reset SHA值  让单个文件退回到指定版本
```

四. 回滚区别(reset与revert)

共同点：都可用回滚

不同点：
 reset 可回滚到之前某个提交（可能是批量回滚），删除被回滚的log，另外不可回滚到已 push 过的的提交，否则回滚后当前版本低于远程版本，被禁止提交。
 使用场景：对本地仓库中未push的提交的回滚
 revert 撤销一个提交的同时会创建一个新的提交
 可用于回滚被 push 过的提交。被回滚的版本git log不会被删除。
 使用场景：你在追踪一个 bug，然后你发现它是由一个提交造成的，这时候撤销就很有用。与其说自己去修复它，然后提交一个新的快照，不如用 git revert，它帮你做了所有的事情。

五. 当前文件与本地仓库文件比较区别

```
git diff 比较的是工作目录树与暂存区之间的区别
git diff --staged 比较的是暂存区和版本库最后一个版本的区别。
git diff HEAD 比较的是工作目录树（包括暂存的和未暂存的修改）与版本库最后一个版本的差别。
```

六. 保持本地仓库与远程仓库分支同步的情况

```
1 git remote show origin （显示本地与远程仓库所有分支情况）如远程分支删除后，本地还有，用 git branch -a 是看不出远程仓库分支已经被删除
    
2 git remote prune origin --dry-run （该命令显示本地无效分支）
    
3 git remote prune origin （同步本地与远程仓库变更（所有分支），执行后可用git branch -a查看）
    
4 git fetch origin [branch]
```

七. Git 客户端工具

可视化操作工具推荐使用：[Sourcetree]([https://www.sourcetreeapp.com/](https://www.sourcetreeapp.com/))