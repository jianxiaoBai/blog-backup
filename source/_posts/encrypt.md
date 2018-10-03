---
title: 简单博文加密
date: 2017-02-07 10:16:31
categories: [Hexo]
tags: [encrypt,Next+Hexo,博客搭建]
comments: false
description: 本文访问密码：123456
password: 123456
---

> 即使是最简单的加密方式也足以阻止90%的访问者

<!-- more -->

### 原理
由于 Hexo 最终编译出来的是静态文件，也就意味着文章的所有信息会原封不动展示在页面中，当你输入一篇文章的地址，所有的内容就已经跟随网络传输过来了。那么博客使用加密是怎么实现的呢？

这就要讲到 js 的阻塞机制了，当调用 `alert();` 函数的时候，整个页面会停止运行，直到你点击确定之后，才会继续执行下去。我们这里需要的也是这样一个假象，阻止整个页面的渲染，直到你输入了正确的密码才能让页面继续渲染实际的文章。可是 `alert();` 只有提醒的功能，没有输入的功能，所以，这里要用到的是 `promt()` 方法。

### promt()方法介绍
这个 promt() 方法有什么作用呢？查看js文档可以知道：
<p id="border-red">**prompt()方法 :**   *用于<u>显示可提示用户进行输入的对话框</u>。
如果用户单击提示框的 **取消** 按钮，则返回 **null**。
如果用户单击 **确认**按钮，则返回 **输入字段当前显示的文本**（用户输入的文本）。*</p>   我们就是利用 `promt()` 方法可以返回用户输入的文本这个特性，获取到返回数据，与我们设置的密码进行验证，从而实现文档加密的。。

### 实践
找到 `themes\next\layout\_partials\head.swig` 文件。
在 &lt;meta&gt; 标签之后添加以下代码：
```
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码','') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```
这里有必要解释一下 `page.password` 是什么东西。以下我给出这篇文章的头部参考：
首先 `page` 是一个变量，你可以理解为这篇文章。以下面的代码为参考，那么 `page.title = 最简单的翻墙方法; page.comments = fasle;`（很好理解吧）

```
title: 最简单的翻墙方法
date: 2017-03-01 12:01:05
tags: [翻墙,hosts]
categories: [外面的世界]
keywords: 翻墙,hosts
comments: false
```

所以，要想加密博文，我们要为文章加上 `password` 属性。`description` 属性用于对文章进行描述。（加密下显示内容）
```
description: 文章访问密码：password
password: password
```

### 总结
这种方式只能说是一点小技巧的应用吧，在大神面前可能不管用，但足以阻挡大多数用户。
更完美的博文加密方式请参考：[加密博客内容，使用密码访问](http://edolphin.site/2016/05/31/encrypt-post/)
