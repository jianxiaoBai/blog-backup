---
title: git 笔记
date: 2017-11-29 20:46:11
tags: [git, 命令]
categories: [技术笔记]
comments: false
---


## 回退相关操作

### 工作区

| 命令                       | 含义                                     |
| -------------------------- | ---------------------------------------- |
| git checkout -- `file.txt` | 丢弃 file.txt 中的修改                   |
| git reset --hard           | 重设暂存区所有文件并且还原工作区所有修改 |

### 暂存区

| 命令                 | 含义                                     |
| -------------------- | ---------------------------------------- |
| git reset            | 重设暂存区所有文件                       |
| git reset HEAD       | 撤销最新一次的 add 状态                  |
| git reset `file.txt` | 把 `file.txt` 的放入工作区               |
| git reset --hard     | 重设暂存区所有文件并且还原工作区所有修改 |

<!-- more -->


### 版本区

 - HEAD => 当前版本
 - HEAD^ => 上一个版本
 - HEAD^^ => 上上一个版本
 - HEAD~100 => 上一100个版本

| 命令                   | 含义                                                                |
| ---------------------- | ------------------------------------------------------------------- |
| git revert HEAD        | 撤销最新一次的 commit ,分支没有改动文件才能执行(存在该commit记录)   |
| git reset `<commit>`   | 删除最新一次的 commit 并且重设暂存区所有文件 (不存在该commit版本号) |
| git reset HEAD~2       | 将当前分支倒退两个提交(高危操作)                                    |
| git reset --hard HEAD^ | 回退上一个版本                                                      |


### 删除文件

| 命令                       | 含义                                                |
| -------------------------- | --------------------------------------------------- |
| rm `file.txt`              | 删除文件                                            |
| git checkout -- `file.txt` | 撤销删除 (未commit之前)                             |
| git clean -n               | 查看那些未被跟踪文件会被移除                        |
| git clean -f               | 移除当前目录下未被跟踪的文件                        |
| git clean -df              | 移除未跟踪的文件以及目录                            |
| git clean -f `<path>`      | 移除未跟踪的文件，但限制在某个路径下                |
| git clean -xf              | 移除当前目录下未跟踪的文件，以及 Git 一般忽略的文件 |


## 创建与合并分支

| 命令                         | 含义                              |
| ---------------------------- | --------------------------------- |
| git checkout -b `dev`        | 创建并且切换到dev分支             |
| git checkout -b `hotfix` dev | 基于dev分支创建一个hotfix分支     |
| git checkout `dev`           | 切换到dev分支                     |
| git branch                   | 查看所有的分支                    |
| git branch -d `dev`          | 删除dev分支(当前分支无法自行删除) |
| git branch -D `dev`          | 强制删除分支(用于未合并分支)      |
| git merge `dev`              | 将dev分支合并到当前分支           |

## 远程仓库的操作

| 命令                               | 含义                                         |
| ---------------------------------- | -------------------------------------------- |
| git remote -v                      | 查看远程仓库的详细信息                       |
| git remote add `remote-name` `URL` | 添加远程仓库                                 |
| git push `origin` `master`         | 将内容提交到远程仓库 origin 的 master 分支上 |
| git remote rm `origin`             | 将远程仓库 origin 删除                       |
| git remote rename `origin` `pb`    | 将远程仓库 origin 改为 pb                    |
| git clone `URL`                    | 克隆一个远程仓库，这里的URL是远程仓库的地址  |
| git pull origin                    | 将远程仓库中更新的数据拉到本地               |
| git pull origin dev                | 拉取远程仓库dev分支到本地                    |
| git push origin aaa                | 将 aaa 分支推送到远程仓库                    |
| git pull --rebase `URL`            | git rebase 代替 git merge 合并本地分支       |
| git push --force                   | 强制推送                                     |



## git commit

| 命令                             | 含义                                            |
| -------------------------------- | ----------------------------------------------- |
| git commit --amend               | 和上一次 commit 合并,并在该基础上编辑commit信息 |
| git commit --amend --no-edit     | 和上一次 commit 合并, 不编辑信息                |
| git commit -a -m "some modified" | git add -A && git commit -m 'some modified'     |


## git rebase

>`<base>` 是可以使任何类型的提交引用（ID/分支名/标签/HEAD）

[参考文档1](https://github.com/geeeeeeeeek/git-recipes/wiki/5.1-%E4%BB%A3%E7%A0%81%E5%90%88%E5%B9%B6%EF%BC%9AMerge%E3%80%81Rebase-%E7%9A%84%E9%80%89%E6%8B%A9)

[参考文档2](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)


 - 什么是 git rebase(变基==改变基线)
   + 变基是将分支移到一个新的基提交的过程, 产生的是一个快速向前的合并以及完美的线性历史
   + rebase 就好像是说「我想将我的更改建立在其他人的进展之上」
   + 绝不要在公共的分支上使用它


  - git rebase `<base>` => git merge `<base>` 相似
  - git rebase --continue => 解决冲突后可执行的命令
  - git rebase --abort => 终止rebase的行动，并且所在分支会回到rebase开始前的状态。
  - git rebase -i `<base>` => 交互式
    + fixup
    + startq
    + squash

## git reflog

> Git 用引用日志这种机制来记录分支顶端的更新

 - git reflog => 显示本地仓库的引用日志
 - git reflog --relative-date => 用相对的日期显示引用日志
 - git reset --hard `0254ea7` 配合使用

注: *引用日志提供的安全网只对提交到本地仓库的更改有效，而且只有移动操作会被记录*

## 其他操作

 - git log --oneline => 简洁的显示 log 记录


## 报错处理

  - [fatal: Unable to create 'project_path/.git/index.lock': File exists.](http://stackoverflow.com/questions/9282632/git-index-lock-file-exists-when-i-try-to-commit-but-cannot-delete-the-file)
    + 删除该分支  =>  `rm -f .git/index.lock`
  - error: failed to push some refs to `<URL>`
    + 第一种解决方式:
      * 强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容 => `git push -f `
    + 第二种解决方式:
      * 这条命令等于合并远程分支，合并完成之后同目录会出现README.md目录 => git pull --rebase origin master
  - fatal: remote origin already exists
    + 先删除远程 Git 仓库 => git remote rm origin
    + 再添加远程 Git 仓库 => git remote add origin `<URL>`
      * 如果执行 git remote rm origin 还报错的话
      * 我们可以手动修改 gitconfig 文件的内容 => vi .git/config
      * 把 [remote “origin”] 那一行删掉就好了。
  - error: src refspec master does not match any
    + 引起该错误的原因是，目录中没有文件，空目录是不能提交上去的
    + touch README => git add README => git commit -m 'first commit' => git push origin maste
  - 用 `git reset --hard` 命令导致目录下所有文件全部被清除。
    + 首先用 `git reflog` 命令查找到对应的sha值,如：cd7b575
    + 通过“git reset --hard  cd7b575”
      * 注意：第二步操作有时会报错, 如: fatal: Unable to create 'D:/chenjunjun/.git/index.lock': File exists.
      * 需要手动删除.git目录下的index.lock文件

