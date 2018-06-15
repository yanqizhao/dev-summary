# Git

## Git Pro学习笔记

[中文v2版](https://git-scm.com/book/zh/v2/)
[中文v1版](https://git-scm.com/book/zh/v1/)

### git add

* git add . 将所有文件的改动从工作区添加入暂存区
* git add file 将某个文件的改动从工作区添加入暂存区

### git commit

* git commit 进入编辑器中添加此次提交的描述信息
* git commit -m 直接输入提交信息，不加引号只会保留第一个词
* git commit -a git add与git commit，会进入编辑器中添加此次提交的描述信息
* git commit -a -m 添加当前改动到暂存区并提交

### git status

* git status -s(--short) 查看当前分支所有改动的简介信息（包括未暂存与已暂存）
    ?? 未暂存
    A  已暂存的文件添加 绿色
    M  已暂存的文件修改 绿色
     M 未暂存的文件修改 红色
    MM 包括已暂存与为暂存的文件修改 左绿右红
    D  已暂存的文件删除 绿色
     D 未暂存的文件删除 红色
    R  已暂存的文件重命名 绿色

* git status -v 详细列出当前的修改

### git rm

* git rm 删除某个文件，之后运行git commit即可保存删除记录的更改
* git rm -f 当某个文件还有在工作区中的改动为存入暂存区或者还有在暂存区中的改动没有被提交时，rm操作需要force强制执行才可成功
* git rm --cached 当某个文件被误提交，不想再追踪，但仍想保留在文件目录中时，使用--cached参数可达到与添加到.gitignore文件中相同的作用，即忽略当前文件的改动，不再追踪；当某个文件的修改只是被存入暂存区而未提交时，使用--cached参数可达到git reset命令相同的作用，即将当前文件的修改放回工作区

### git mv

* git mv 相当于mv a b, git rm a, git add b

### git reset

### git log

### git diff

* git diff 查看未暂存的改动
* git diff --cached(--staged) 查看已暂存的改动
* git difftool --tool-help 查看当前电脑中支持的diff工具

### git merge

### git rebase

