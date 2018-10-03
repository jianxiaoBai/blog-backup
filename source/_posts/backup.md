---
title: HEXO+GitHub,搭建博客 - 备份
date: 2017-03-07 16:26:30
tags: [Next+Hexo,博客搭建,backup]
categories: [Hexo]
comments: false
---


<!-- <div class="out-img-topic">![Markdown](http://on5pjsxrv.bkt.clouddn.com/backup.png)</div> -->

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用Hexo写博客是一件比较享受的事情，无奈如果换电脑或者系统崩了的话，你就会一脸懵B了，备份博客就显得尤为重要。先说说我的感受，博客刚搭建好的时候就想过这个问题，那时候对 `git` 似懂非懂吧。在网上找了很多教程方法，大概就是说要创建一个分支来存放 blog 文件，但是翻腾来翻腾去还是没有搞定。<!-- more --> 最后索性简单粗暴点，<u>**在 GitHub 上创建一个仓库，把 blog 文件整个打包上传。**</u>使用过程中发现这个方法还不错，至少对于小白来说很容易理解，也很难出错，就一直沿用到现在。

<p id="border-red">这种方式虽然能够备份 Hexo 博客的源文件，但是对于博主这种懒人，每次更新博文都需要输入两三行重复的Git命令真是一件麻烦的事情。</p>

### 自动备份
#### 准备
本方法需要提前将 Hexo 加入 Git仓库 并与 Github 远程仓库绑定之后，才能正常工作。
**具体做法可以参考：**[上传本地项目到GitHub](https://cwyaml.github.io/2017/01/08/update%20to%20github/)
#### 安装 shelljs 模块
要实现这个自动备份功能，需要依赖 `NodeJs` 的一个 `shelljs 模块`,该模块重新包装了 child_process,调用系统命令更加的方便。
使用以下命令，完成 shelljs 模块的安装：
```
npm install --save shelljs

```
#### 编写自动备份脚本
待到模块安装完成，在**Hexo根目录** 的 **scripts**文件夹下新建一个js文件，文件名随意取。
***如果没有scripts目录，请新建一个。***
```
require('shelljs/global');
try {
    hexo.on('deployAfter', function() {//当deploy完成后执行备份
        run();
    });
} catch (e) {
    console.log("产生了一个错误<(￣3￣)> !，错误详情为：" + e.toString());
}
function run() {
    if (!which('git')) {
        echo('Sorry, this script requires git');
        exit(1);
    } else {
        echo("======================Auto Backup Begin===========================");
        cd('C:/Blog');    //此处修改为Hexo根目录路径
        if (exec('git add .').code !== 0) {
            echo('Error: Git add failed');
            exit(1);
        }
        if (exec('git commit -m "Form auto backup script\'s commit"').code !== 0) {
            echo('Error: Git commit failed');
            exit(1);
        }
        if (exec('git push origin master').code !== 0) {
            echo('Error: Git push failed');
            exit(1);
        }
        echo("==================Auto Backup Complete============================")
    }
}
```
<span id="inline-red">注意：</span>
- 其中，需要修改第17行的 `D:/hexo` 路径为 `Hexo的根目录` 路径。（脚本中的路径为博主的Hexo路径）
- 如果你的Git远程仓库名称不为 `origin` 的话，还需要修改第28行执行的push命令，修改成自己的远程仓库名和相应的分支名。

### 测试
保存脚本并退出，然后执行 `hexo d` 命令，将会得到类似以下结果:
```
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
......
======================Auto Backup Begin===========================
warning: LF will be replaced by CRLF in package.json.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/hexo1.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/update to github.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/wangyimusic.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in themes/next-5.0.1/layout/_partials/head.swig.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/backup.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/encrypt.md.
The file will have its original line endings in your working directory.
[master 1bb6cc5] Form auto backup script's commit
 Committer: unknown
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 6 files changed, 177 insertions(+), 2 deletions(-)
 create mode 100644 scripts/autobackup.js
 create mode 100644 source/_posts/backup.md
 create mode 100644 source/_posts/encrypt.md
To https://github.com/cwyaml/blog-backup.git
   d7bc718..1bb6cc5  master -> master
==================Auto Backup Complete============================
```
这样子，每次更新博文并 deploy 到服务器上之后，备份就自动启动并完成备份啦~是不是很方便呢？

**Enjoy it！**

> 参考：wanghao大神 [自动备份Hexo博客源文件](https://notes.wanghao.work/2015-07-06-%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BDHexo%E5%8D%9A%E5%AE%A2%E6%BA%90%E6%96%87%E4%BB%B6.html)
