---
title: Markdown 使用中常见的问题及解决方法
date: 2017-02-14 21:05:06
tags: [Markdown]
categories: [零七碎八]
comments: false
---


<!-- <div class="out-img-topic">![Markdown](http://on5pjsxrv.bkt.clouddn.com/markdown.jpg)</div> -->

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;就像 Markdown 官方文档里描述的一样：***可读性，无论如何，都是最重要的。*** Markdown 的目标是实现 &nbsp; <span id="yu-1">『易读易写』</span> 。&nbsp;Markdown 从发布到现在备受好评，经过这一段的使用，整体感觉挺顺手，不过还是存在很多问题，所以总结一下喽。。

<!-- more -->

## 编辑器
其实自己喜欢的才是最好的。(像 vim 、emacs什么的不推荐，因为我也不会用)
### MAC平台
自己没用过，不做推荐。你可以看看这个帖子：[Mac 上适合码农用的 Markdown 编辑器是什么？](https://www.zhihu.com/question/28886671)

### Windows平台
#### 印象笔记
[马克飞象传送门](https://maxiang.io/)，界面不是很好看，书写的时候感觉很别扭

#### 有道云笔记
[有道传送门](http://note.youdao.com/)，同样很丑，强迫症受不了

#### Sublime Text
强大的 Sublime Text 总是能给我们很多惊喜，经过各种对比，sublime 满足了我对审美的要求。首先我们需要安装两个插件：（至于怎么安装就不说了）
- **markdownEditing** 用来书写
- **markdownPreview** 用来预览

#### Atom（强烈推荐）
GitHub 推出的编辑器，界面很好（就是启动有点慢），必须支持一下。默认继承了 markdown 预览，快捷键为 `Ctrl+shift+M`。推荐插件：
- **markdown-preview** 实时预览
- **markdown-scroll-sync**  编辑区和预览区同步滚动
- **markdown-writer** 方便管理图片链接等
- **markdown-table-formatte** 表格格式化

## 使用方法
[Markdown官方文档](http://www.markdown.cn/)

## 常见问题汇总
### html标签显示
比如说我要写一篇博客，标题为“html中 &lt;canvas&gt; 的使用”
```
## html中 <canvas> 的使用
```
如果这样写就会出现排版上的问题（不信你试一下），那么怎么解决呢？其实认真想一下就能明白，Markdown 的语法是基于 html 的，我们直接写 &lt;canvas&gt;，自然会被理解为一个标签，而不是要显示的文本。。所以，问题回归到 html 上。在网页中，我们要显示 &lt;canvas&gt; 时要用到 `转义字符`, 所以 Markdown 中也一样，我们应该这样写：
```
## html中 &lt;canvas&gt; 的使用
```

### 代码语法高亮
这个问题困扰了我好久，官方文档里竟然没有说明！只好自己去查找方法。Markdown 中显示代码块是这样的格式：
![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown01.png)
显示为：
```
    <p>这是一个p标签</p>
```
而我们这样写：
![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown02.png)
就可以实现代码高亮了
``` html
    <p>这是一个p标签</p>
```
据说这种方式一共支持四十多种语言，有兴趣的话你可以研究一下。

### 图片
Markdown 中嵌入图片，如果使用本地图片就要用到 html 标签来引用，这种方法很稳定，但是使文档变得很大（一张图片最少几百k吧）。所以我们要用到 **图床** 。

#### 贴图库
推荐使用 &nbsp; [贴图库](http://www.tietuku.com/)  &nbsp;快速，免费（我使用过程中没掉过链子）

![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown03.png)

注册登录，就可以上传图片，每张图片自动生成 **原图**、**展示图**和 **缩略图**的<u>图片外链</u>、<u>html代码</u>、<u>Markdown外链</u>等。只要把对应的代码粘贴到你的文档中就可以了。。

#### 七牛云存储

这个最近很火，可靠、可扩展、低成本等等有很多优点。你可以试一下。
我们主要用到他的 **对象存储** 服务，创建一个公开仓库，把图片上传就可以生成外链了。


### gif
Github 上的开源项目，ReadMe.md 是也支持 Markdown 语法的，通常会看到很多开源项目的 ReadMe 中有 **动态演示**效果，看到这个项目的人一目了然，非常方便，gif本身也是一种图片格式，在 Markdown 中 *引用时和正常图片的引用一样*，但需要专门的工具生成 gif 格式的图片才行，在这里强烈推荐 [LICEcap](http://www.cockos.com/licecap/)，它是一款 windows 上的录屏软件，录制后保存的格式为 gif，体积小并且同样也可以在图床上生成链接。

![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown04.gif)

### 插入音乐
你可以把音乐文件下载到本地，然后简单粗暴的使用 html 中的 &lt;video&gt; 标签。当然，如果这样就不必写下去了，告诉你简单的方法：**网易云音乐**

打开网易云音乐网页版，搜索自己喜欢的音乐，比如我找到 **告白气球**

![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown05.png)
我们可以看到在图片的下边有一个 `生成外链播放器` , 点击会出现

![Markdown](http://on5sixmz1.bkt.clouddn.com/markdown06.png)
选择 **合适的尺寸** 和 **播放模式** 之后。把下边的代码复制到你的 Markdown 文档中就可以了。试着听一下吧！！(我选择了最小尺寸)

<div style="max-width: 450px;max-height: 180px;margin: 0 auto 40px;"><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=298 height=52 src="//music.163.com/outchain/player?type=2&id=423015566&auto=0&height=32"></iframe></div>
<p id="border-red" style="text-indent: 30px">试着去把一个 **歌单生成外链播放器** 插入到你的 Markdown 中，这样你跟新歌单你的博文也会跟着变化，而不必在想跟换歌曲时头疼<p>
