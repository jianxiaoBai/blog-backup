---
title: HEXO 博客同时部署到 GitHub 和 Coding
date: 2017-03-08 13:08:44
tags: [Next+Hexo, 博客搭建]
categories: [Hexo]
comments: false
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;很多人都把 hexo 托管到 Github 上，因为 Github 大家都用的比较久了。但是，你的博客主要访问者肯定还是国内的用户，国内的用户访问 coding 比 github 是要快不少的。还可以<u>利用域名解析实现国内的走 coding，海外的走 github，</u>分流网站的访问。

<!-- more -->

### 注册 GitHub 和 Coding

[github 官网](https://github.com/) &nbsp;&nbsp;||&nbsp;&nbsp; [Coding 官网](https://coding.net) &nbsp;&nbsp;注册就不必多说，不会的可自行百度。
**需要注意的是**：最好使用同一个 **用户名** 和 **邮箱** ，以免引起不必要的麻烦。

### 创建项目

在 GitHub 上创建项目，名称为：**yourname.github.io**
在 Coding 上创建项目，名称为：**yourname**

### 配置 SSH

配置 shh key 是让本地 git 项目与远程的 github 建立联系

#### 获取 ssh

- 检查是否已经有 SSH Key，打开 `Git Bash`，输入
  ```
  cd ~/.ssh
  ```
- 如果没有 **.ssh** 这个目录，则生成一个新的 SSH，输入

  ```
  ssh-keygen -t rsa -C "your e-mail"
  ```

  <p id="border-red"> **注意:**  此处的邮箱地址，是你注册 **GitHub** 和 **coding** 时的邮箱地址; 此处的**「-C」**的是大写的**「C」** 。</p>

- 接下来几步都直接按回车键,然后系统会要你输入密码 (防止别人往你的项目里提交内容)
  ```
  Enter passphrase (empty for no passphrase):<输入加密串>
  Enter same passphrase again:<再次输入加密串>
  ```
  成功后，我们打开 _C:\Users\cwyaml\.ssh_ 打开 _id_rsa.pub_ 文件。里面的代码就是 ssh key。

#### 添加 SSH Key 到 GitHub 和 Coding

**GitHub 添加方法：**

- 进入 Github 官网，点击头像，再按 _settings_ 进入设置。
- 点击 _New SSH key_ 创建
- title 输入邮箱，key 里面粘贴刚才右击复制的内容,再点 _Add SSH key_ 即可。（会让你输入密码）

![Markdown](http://on5sixmz1.bkt.clouddn.com/github&coding01.png)

**Coding 添加方法：**

- 登录账号后点击 _左侧账户_
- 在点 _SSH 公钥_ 设置即可 。（同样要输入密码）

![Markdown](http://on5sixmz1.bkt.clouddn.com/github&coding02.png)

#### 测试 SSH 是否配置成功

打开 `Git Bash`，首先测试 GitHub 是否成功？输入:

```
ssh -T git@github.com
```

(如配置了密码则要输入密码,输完按回车。)如果显示以下内容，则说明 Github 中的 ssh 配置成功。

```
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```

然后测试 Coding 是否成功？

```
ssh -T git@git.coding.net
```

如果显示以下则说明配置成功：

```
Hello username You've connected to Coding.net by SSH successfully!
```

### 上传博客文件

**修改站点配置文件：**

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:
    github: git@github.com:cwyaml/cwyaml.github.io.git,master
    coding: git@git.coding.net:cwyaml/cwyaml.git,master
```

然后你就可以 `hexo c、hexo g、hexo d` 了。

### 开启 pages 服务

GitHub 已经默认开启，就不必多说了。
Coding 进入对应项目，点击 **代码>pages 服务** ，把部署来源改为 **master** 即可。

![Markdown](http://on5sixmz1.bkt.clouddn.com/github&coding03.png)

### 访问博客

这样我们整个部署过程就完成了。有两个地址可以访问我们的博客：
GitHub pages：https://cwyaml.github.io
Coding pages：https://cwyaml.coding.me
