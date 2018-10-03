---
title: Flex
date: 2017-05-30 15:58:06
tags: [flex]
categories: [技术笔记]
comments: false
---

### Flex 布局是什么

> Flex 是 Flexble Box 的缩写，意为 “弹性布局”，用来为盒状提供最大的灵活性。

**任何一个容器都可以指定为 Flex 布局**

_注意，设为 Flex 布局以后，子元素 的 loadt、cleat 和 vertical-align 将失去作用_

```html
<style type="text/css">
    .box{
      display: flex;
    }

    .box{
    /* 行内元素也可以使用 Flex 布局。 */
      display: inline-flex;
    }
</style>
```

<!-- more -->

#

**flex-flow**简写：这是 flex-direction 和 flex-wrap 两个属性的缩写,默认值是 row nowrap。

### 容器的属性

**主轴方向：flex-direction:**

| 属性值           | 属性作用 |
| ---------------- | -------- |
| row（默认）；    | 从左到右 |
| row-reverse；    | 从右到左 |
| column；         | 从上到下 |
| column-reverse； | 从下到上 |

**flex-wrap：是否换行**

| 属性值           | 属性作用 |
| ---------------- | -------- |
| nowrap（默认）； | 不换行   |
| wrap；           | 正常换行 |
| wrap-reverse；   | 返向换行 |

**justify-content：设伸缩项目在相对 `主轴` 水平上的对齐方式**

| 属性值               | 属性作用                       |
| -------------------- | ------------------------------ |
| flex-start（默认）： | 左对齐                         |
| flex-end：           | 右对齐                         |
| center：             | 居中                           |
| space-between：      | 首尾对齐，项目之间的间隔相等。 |
| space-around：       | 每个项目两侧的间隔相等。       |

**align-content:设伸缩项目在相对 `主轴` 水垂直的对齐方式**

| 属性值            | 属性作用                                                                 |
| ----------------- | ------------------------------------------------------------------------ |
| flex-start：      | 上对齐                                                                   |
| flex-end：        | 下对齐                                                                   |
| center：          | 居中                                                                     |
| space-between：   | 首尾对齐，项目之间的间隔相等。                                           |
| space-around：    | 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。 |
| stretch（默认）： | 每个项目两侧的间隔相等。                                                 |

**align-items：管理伸缩容`器侧轴方向`的额外空间**

| 属性值               | 属性作用                                              |
| -------------------- | ----------------------------------------------------- |
| flex-start（默认）： | 左对齐                                                |
| flex-end：           | 右对齐                                                |
| center：             | 居中                                                  |
| baseline:            | 项目的第一行文字的基线对齐。                          |
| stretch（默认）：    | 如果项目未设置高度或设为 auto，将占满整个容器的高度。 |

#

### 项目的属性

| 属性作用    | 属性值说明                                                             |
| ----------- | ---------------------------------------------------------------------- |
| order       | 数值越小，排列越靠前，默认为 0。                                       |
| flex-grow   | 定义一个 Flex 项目的扩大比例，默认为 0                                 |
| flex-shrink | 定义一个 Flex 项目的缩小比例，默认为 0                                 |
| flex-basis  | 定义了 Flex 项目在分配 Flex 容器剩余空间之前的一个默认尺寸，类似 width |

**align-self：管理伸缩容`器侧轴方向`的额外空间**

| 属性值               | 属性作用                                              |
| -------------------- | ----------------------------------------------------- |
| auto                 | 自动                                                  |
| flex-start（默认）： | 左对齐                                                |
| flex-end：           | 右对齐                                                |
| center：             | 居中                                                  |
| baseline:            | 项目的第一行文字的基线对齐。                          |
| stretch（默认）：    | 如果项目未设置高度或设为 auto，将占满整个容器的高度。 |

**简写：flex**

flex 是 flex-grow，flex-shrink，flex-basis 三个属性的缩写。第二个和第三个参数是可选值。默认值是 0 1 auto。

`建议使用缩写属性。如果flex取值为none，等于0 0 auto。`

[参考一](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
[参考二](http://www.cnblogs.com/fxycm/p/4649648.html)
