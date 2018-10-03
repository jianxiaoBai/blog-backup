---
title: HEXO+GitHub,搭建博客 - 域名绑定
date: 2017-03-09 14:50:21
tags: [Next+Hexo,博客搭建,域名]
categories: [Hexo]
comments: false
---


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;前面也讲过了，我们把博客同时托管到 Github 和 Coding。我们就有两个域名可以访问站点，但是又出现几个问题：使用的不是自己的域名；两个地址的统计信息（文章阅读量，访问量等）相互独立，不能合并；github pages国内访问速度慢（300ms左右，毕竟国外的服务器）。

这就有点坑爹了，强迫症怎么能忍。下面给出解决方法。。

<!-- more -->

<span id="inline-blue">解决方案：</span>
我们知道 github 和 coding 的 pages 服务都提供 `自定义域名` 功能。我们可以利用这一点，绑定自己的域名。域名解析的时候实现国内访问 coding pages ，国外访问 github pages ,从而加快访问速度。 具体怎么实现，往下看：

### 购买域名
首先我们要购买一个域名，推荐到 [万网](https://wanwang.aliyun.com/?spm=5176.8142029.388261.25.nvxK84) 购买。（毕竟很方便）
具体步骤可以参考这篇文章：[万网域名注册教程](http://jingyan.baidu.com/article/4853e1e513d0061908f7265b.html)。

<p id="border-red">购买域名一定要实名认证，否则会停止解析</p>

### 域名解析
这一步是最重要的，我们要把域名指向 github 和 coding 的服务器空间。
- 登录阿里云，进入 **控制台** 。依次点击 **域名与网站** > **云解析DNS** 就会出现你购买的域名信息

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D01.png)

- 点击 **解析**，然后按照下图依次添加解析：（这张图片可以放大）

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D02.png)


<p id="border-red">从上图可以看出，我们的解析实现了分流。国内线路访问Coding pages，国际线路访问Github Pages。</p>

### 托管平台设置
#### Coding平台
进入对应项目的 pages 设置页面（项目 > 代码 > pages服务）

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D04.png)

成功后会显示：

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D03.png)


#### Github平台
进入对应项目的 pages 设置页面（setting > github pages > Custom domain)

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D05.png)

成功后会显示：

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D06.png)

到此我们的博客就可以正常运行了！！

### 总结
一切搞定后，在回头看一下我们的问题：
**@** 两个地址的统计信息（文章阅读量，访问量等）相互独立，不能合并；
> 从两个地址访问都会跳转到我们绑定的域名。统计信息自然也是绑定后域名的信息。

**@** github pages国内访问速度慢（300ms左右）
> 我们测试一下 Ping：(表现不错)

![Markdown](http://on5sixmz1.bkt.clouddn.com/%E5%9F%9F%E5%90%8D07.png)

**@** 托管平台给出的二级域名太丑。
> 不存在的.....

