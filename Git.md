# Git

## book

[Git菜单](https://github.com/geeeeeeeeek/git-recipes)
[Git Pro中文v2版](https://git-scm.com/book/zh/v2/)
[Git Pro中文v1版](https://git-scm.com/book/zh/v1/)
[易百教程-Git教程](https://www.yiibai.com/git/)
[19 Git Tips For Everyday Use](https://www.alexkras.com/19-git-tips-for-everyday-use/)
[每天使用Git的19小技巧](https://lighter.github.io/2016/07/09/19-tips-for-everyday-git-use/)

## Git Pro中文v2版学习笔记

### git checkout

* git checkout -- \<文件名\> 撤销对文件的修改，即将对未存入暂存区仍在工作区的某个文件的修改撤销掉
* git checkout \<分支名\> 检出分支
* git checkout -b \<分支名\> 创建并检出分支
* git checkout --track origin/\<branch name\> 检出分支并关联远程分支

### git add

* git add . 将所有文件的改动从工作区添加入暂存区
* git add file 将某个文件的改动从工作区添加入暂存区

### git commit

* git commit 进入编辑器中添加此次提交的描述信息
* git commit -m 直接输入提交信息，不加引号只会保留第一个词
* git commit -a git add与git commit，会进入编辑器中添加此次提交的描述信息
* **git commit -a -m 添加当前改动到暂存区并提交**
* **git commit --amend 将本次修改同上次一同提交并修改提交信息**

### git status

* **git status -s(--short) 查看当前分支所有改动的简介信息（包括未暂存与已暂存）**
    ?? 未暂存
    A  已暂存的文件添加 绿色
    M  已暂存的文件修改 绿色
     M 未暂存的文件修改 红色
    MM 包括已暂存与为暂存的文件修改 左绿右红
    D  已暂存的文件删除 绿色
     D 未暂存的文件删除 红色
    R  已暂存的文件重命名 绿色

* **git status -v 详细列出当前的修改**

### git rm

* git rm 删除某个文件，之后运行git commit即可保存删除记录的更改
* git rm -f 当某个文件还有在工作区中的改动未存入暂存区或者还有在暂存区中的改动没有被提交时，rm操作需要force强制执行才可成功
* git rm --cached 当某个文件被误提交，不想再追踪，但仍想保留在文件目录中时，使用--cached参数可达到与添加到.gitignore文件中相同的作用，即忽略当前文件的改动，不再追踪；当某个文件的修改只是被存入暂存区而未提交时，使用--cached参数可达到git reset命令相同的作用，即将当前文件的修改放回工作区

### git mv

* git mv 相当于mv a b, git rm a, git add b

### git reset

* **git reset HEAD\<文件名\> 取消暂存的文件，即将存入暂存区的某个文件放回工作区**

### git log

* git log 显示每次提交的hash值与提交信息
* **git log -\<n\> 显示最近的n次提交**
* **git log -p 显示每两个相邻commit之间的差异**
* git log -p -2 显示最近两个相邻commit之间的差异
* git log --decorate 显示commit与分支的对应关系
* git log --stat 显示每次提交改动的文件名与文件个数
* git log --shortstat 显示每次提交改动的文件个数
* git log --name-only 只显示每次提交的文件名
* git log --name-status 显示每次提交的文件名与文件状态（增删改）
* **git log --abbrev-commit 显示hash值的前9位**
* git log --relative-date 使用较短的时间格式（几分钟前，几小时前等）
* **git log --pretty=oneline 每个提交用一行显示**
* git log --pretty=short 默认的显示
* git log --pretty=full 显示author与commiter
* git log --pretty=fuller 显示提交与修改的日期
* git log --pretty=format 格式化显示提交信息

    常用选项：
    
    * %H 提交对象（commit）的完整哈希字串
    * %h 提交对象的简短哈希字串
    * %T 树对象（tree）的完整哈希字串
    * %t 树对象的简短哈希字串
    * %P 父对象（parent）的完整哈希字串
    * %p 父对象的简短哈希字串
    * %an 作者（author）的名字
    * %ae 作者的电子邮件地址
    * %ad 作者修订日期（可以用 --date= 选项定制格式）
    * %ar 作者修订日期，按多久以前的方式显示
    * %cn 提交者（committer）的名字
    * %ce 提交者的电子邮件地址
    * %cd 提交日期
    * %cr 提交日期，按多久以前的方式显示
    * %s 提交说明
* **git log --graph 显示提交的节点图**
* git log --since=2.days 显示最近两天的提交
* git log --after=2018-06-21 显示6.21之后的提交
* git log --until 显示直到某个时间点的提交
* git log --before 显示某个时间点之前的提交
* git log --grep 搜索关键字
* git log --all-match 匹配多个条件
* git log -S 仅显示添加或移除了某个关键字的提交

### git diff

* **git diff 查看未暂存的改动**
* **git diff --cached(--staged) 查看已暂存的改动**
* git difftool --tool-help 查看当前电脑中支持的diff工具

### git remote

* git remote 获取远程仓库名称
* git remote -v 获取远程仓库读写的名称
* git remote add \<shortname\> \<url\> 添加远程仓库并设定简写名称
* git remote rename \<old name\> \<new name\>
* git remote rm \<name\> 移除远程仓库
* **git remote show \<仓库名称\> 显示远程仓库分支信息**

    > 这个命令列出了当你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 git pull 时哪些分支会自动合并。

### git tag

* **git tag 列出所有tag**
* **git show 5.28 查看对应tag的提交信息**
* **git tag -l '5.2*' 列出符合引号内格式的tag**
* git push origin 5.28 将tag推向远程
* git push origin --tags 一次性推送多个tag
* git checkout -b feature/r5.28 5.28 检出5.28tag到feature/r5.28分支fb

###### 附注tag

* **git tag -a 5.28 -m 'version 5.28' 标注tag**
* **git tag -a 5.28 \<hash\> -m 'xxx' 补打tag** 

###### 轻量tag

* git tag 5.28 不加标注信息直接打tag

### git branch

* git branch \<branch name\> 创建分支
* git branch -v 查看每个分支的最后一次提交
* git branch -vv 同上，并会显示每个分支与远程分支的关联状态
* git branch --merged 已经合并到当前分支的分支
* git branch --no-merged 未合并到当前分支的分支
* git branch -u origin/\<branch name\> 设置关联远程分支
* git branch --set-upstream-to origin/\<branch name\> 设置关联远程分支 

### git fetch

* git fetch
* git fetch --all

### git push

* git push origin --delete \<branch name\> 删除远程的分支

### git merge

### git rebase

* **git rebase --onto \<branch 1\> \<branch 2\> \<branch 3\> 将branch 3与branch 2共同父节点后branch 3的改变合并入branch 1中**
* git rebase \<branch 1\> \<branch 2\> 将branch 2使用`rebase`合并入branch 1中


