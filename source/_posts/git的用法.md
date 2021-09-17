---
title: git的用法
date: 2021-07-07 19:59:53
tags: git
categories: daily
---

git 的笔记
<!--more-->
git 管理的是修改
工作区里面有个不可见的.git文件， 是这一项目的版本库，版本库里有缓存区和分支

git add 添加文件到缓存区
git commit -m "" 把添加到缓存区到文件全部提交到master分支
git push 把提交到文件推送到远程仓库
git status 查看仓库状态
git diff 文件 查看该文件的修改
git log 查看修改日志
git checkout -- [file] 把文件在工作区(就是目录or在编辑器里面)做的修改全部撤销
git reset HEAD [file] 当做了某修改并且 git add 后使用撤销

+ git恢复版本(作出修改并git commit 后修改的方法)
```
用git commit 多次提交不同版本

git log --pretty=oneline 简要查看日志

git reset --hard HEAD^ 恢复到上一个版本 三个以上用 HEAD～3 (HEAD表示当前版本)

git reset --hard commit Id  恢复到指定版本号

git reflog 


```

+ 在工作区删文件

    git rm [file] 在版本库里删除文件
    git checkout -- [file] 

    
# 关于分支
我们已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
![](./git的用法/0.png)
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。

当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
![](git的用法/l.png)
你看，Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
![](git的用法/l-1.png)

假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：

![](git的用法/0-1.png)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：

![](git的用法/0-2.png)

+ 实战代码

    ```
    首先，我们创建dev分支，然后切换到dev分支：
    git checkout -c dev

    git switch命令加上-b参数表示创建并切换，相当于以下两条命令：
    git branch dev
    git switch dev

    然后，用git branch命令查看当前分支：
    git branch

    git branch命令会列出所有分支，当前分支前面会标一个*号。

    然后，我们就可以在dev分支上正常提交

    dev分支的工作完成，我们就可以切换回master分支
    git switch master

    我们把dev分支的工作成果合并到master分支上：
    git merge dev

    删除dev分支
    git branch -d dev
    ```

+ 总结

```
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name>

创建+切换分支：git checkout -b <name>或者git switch -c <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

```

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用git log --graph命令可以看到分支合并图

` git log --graph --pretty=oneline --abbrev-commit`

+ 分支策略:


在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![](git的用法/0-3.png)

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。


 `git stash` 把当前工作现场“储藏”起来，等以后恢复现场后继续工作

 场景：出现了一个Bug，但当前分支的工作还未完成提交，却必须马上修复bug,此时就用该命令

`git stash list` 查看隐藏的现场
`git stash pop` 恢复工作现场
`git cherry-pic <commmit>` 让我们能复制一个特定的提交到当前分支(多个分支修改同一个Bug，即在某一个分支修改完成后直接用该命令复制给其他人)

+ feature分支用于开发某一项新需求， issue-n 分支用于改bug

+ `git push origin <branch>` 推送分支

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用`git remote -v`

多人协作的工作模式通常是这样：

首先，可以试图用`git push origin <branch-name>`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

`git rebase` 可以把本地未push的分叉提交历史整理成直线,目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。


# 关于标签
标签就是某个版本的快照 和某个commit绑定在一起
`git tag <name>` 制作标签

`git tag` 查看所有标签

标签默认打在当前版本号上，如果想要打到以前的版本上，找到之前版本的commit id `git tag <name> <commit id>`

`git show <tagname>` 查看详细的标签信息

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
`git tag -a v0.1 -m "version 0.1 released" 1094adb`

`git tag -d v0.1` 删除标签

`git push origin <tagname` 推送某个标签到远程仓库

`git push origin --tags` 一次性推送

从远程删除标签
1. 先本地删除 ` git tag -d v0.9`
2. 再远程删除 命令也是push `git push origin :refs/tags/v0.9`


