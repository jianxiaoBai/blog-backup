---
title: Hexo 笔记
date: 2017-07-06 16:59:12
tags: [快捷键]
categories: [Hexo]
comments: false
---

## 基本操作

### 常用命令

| 命令              | 说明                          |
| ----------------- | ----------------------------- |
| hexo n "我的博客" | hexo new "我的博客" #新建文章 |
| hexo p            | hexo publish                  |
| hexo g            | hexo generate#生成            |
| hexo s            | hexo server #启动服务预览     |
| hexo d            | hexo deploy#部署              |

<p id="border-purple">hexo d #部署 #可与hexo g合并为 `hexo d -g`</p>

<!-- more -->

### 服务器

| 命令                       | 说明                                             |
| -------------------------- | ------------------------------------------------ |
| hexo server                | #Hexo 会监视文件变动并自动更新，您无须重启服务器 |
| hexo server -s             | #静态模式                                        |
| hexo server -p 5000        | #更改端口                                        |
| hexo server -i 192.168.1.1 | #自定义 IP                                       |
| hexo clean                 | #清除缓存 网页正常情况下可以忽略此条命令         |


### 完成后部署

| 命令   | 说明          |
| ------ | ------------- |
| hexo g | #生成静态网页 |
| hexo d | #开始部署     |

### 模版

| 命令                     | 说明                                                    |
| ------------------------ | ------------------------------------------------------- |
| hexo new "postName"      | #新建文章                                               |
| hexo new page "pageName" | #新建页面                                               |
| hexo generate            | #生成静态页面至public目录                               |
| hexo server              | #开启预览访问端口（默认端口4000，'ctrl + c'关闭server） |
| hexo deploy              | #将.deploy目录部署到GitHub                              |


### 写作

| 变量     | 描述                       |
| -------- | -------------------------- |
| :title   | 标题                       |
| :year    | 建立的年份（4 位数）       |
| :month   | 建立的月份（2 位数）       |
| :i_month | 建立的月份（去掉开头的零） |
| :day     | 建立的日期（2 位数）       |
| :i_day   | 建立的日期（去掉开头的零） |

**示例：**

```
title: 搭建个人博客
layout: post
date: 2020-01-01 12:00:00
comments: true
categories: Blog
tags: [Hexo]
words: Hexo, Blog
description: 搭建个人博客还是要用 Hexo
```


---------


## 书写语法


### 自定义图片大小
```
标准：{% img [class names] /path/image [width] [height] [title text [alt text]] %}
例如：{% img  /imgs/baiyan.jpg 100 50  %}
```


### 突破容器宽度限制的图片的三种方式

<p id="border-blue">当使用此标签引用图片时，图片将自动扩大 26%，并突破文章容器的宽度。 此标签使用于需要突出显示的图片, 图片的扩大与容器的偏差从视觉上 `提升图片的吸引力` 。</p>

```
<img src="/image-url" class="full-image" />
{% fullimage /image-url, alt, title %}
{% fi /image-url, alt, title %}
```

### Bootstrap Callout

```
{% note class_name %} Content (md partial supported) {% endnote %}
```


其中，`class_name` 可以是以下列表中的一个值：

 - default  默认
 - primary  提示
 - success  成功
 - info     提示
 - warning  警告
 - danger   危险


### 从书中引用


{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
新：DevDocs现在附带语法高亮。http://devdocs.io
{% endblockquote %}


### 文本居中的引用3中方式

```
<blockquote class="blockquote-center">blah blah blah</blockquote>
{% centerquote %}blah blah blah{% endcenterquote %}
{% cq %}人一切的痛苦，本质上都是对自己的无能的愤怒{% endcq %}   推荐
```
