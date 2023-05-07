---
aliases: 
tags: []
draft: false
categories: []
title: obsidian 与 hugo 的配合食用
date: 2023-05-03T22:41:48+08:00
lastmod: 2023-05-07T22:14:31+08:00
---

# Obsidian Or Typora 

## 为什么选择 obsidan 而不是 typora？

obsidian 支持更多的功能，包括全局图谱、双链、强大的搜索整理功能、以及丰富的生态（官方内置以及社区插件数不胜数）。

更重要的是免费，可本地化使用。但是对于同步及发布功能是需要购买服务的，如果有条件还是建议支持一下官方的。

## 怎么与 Hugo 兼容及搭配？

因为之前就是使用 Hugo + Typora 进行文章编写及发布的，现在换到 Obsidian 可能会有不同。首先就是 Hugo 目前还无法原生的支持解析双链格式，但是这各功能又是 Obsidian 比较核心的功能。目前有不少的解决方案，我选择的是使用 [[../../exampleSite/setup|setup]] 这个项目，是兼容 Hugo 的，基本和 Hugo 的使用方法一致，只是他们对双链进行了处理并且加入了一些新的功能，详细可以查看这个项目或者 fork 下来进行 DIY。

当然，现在这个站点你所看到的效果就是这个项目带来的，当然这只是再编写这个文档的期间，相信 Hugo 或者 Obsidian 社区肯定会有越来越好的方案。那个时候将会是”开箱即用“的。


![[../../images/image-20230507175056062.png]]

## 问题及解决方案

### 图片路径问题

如果是以博客项目为根目录打开的 Obsidian 仓库，那么使用 [[../../exampleSite/setup|setup]] 中的配置应该是没问题的，配置成绝对路径，这种方式比较明了，博客的内容不会跟其他项目掺杂，就是要多配置一份，如果有插件什么的都要配置，但是还好我们可以把 .obsidan 目录直接复制过去重新启动就好了，非常方便。平时的 Git 同步也推荐把 .obsidian 添加到版本控制中。**注意：需要把 content 作为仓库根目录，而不是整个 quartz 项目 ** 

如果是以子模块放置到一个其他的笔记目录中，也就是要把所有的东西都放在一个 Obsidian 仓库中，那样的话渲染图片路径会有问题，可以 fork 项目自己进行调整，或者再 content 中创建 images 目录，然后文章中的图片都使用相对路径，经过测试只能放在 content 目录下才可以正常加载。

如果想放在某篇笔记下的目录是不行的，或者可以放图床，图床的问题是如果你不给每个图片进行标明文件表示的是什么，很容易因为图片过多而变乱，或者图床不稳定导致解析失败。

### 路径问题会显示灰色的链接

如果路径存在问题，或者使用原生的 markdown 格式，如果找不到链接文件，会显示灰色链接并且无法跳转，就像 [百度](www.baid.com) 这样。

如果是外部的链接，需要使用 https:// 或 http:// 带上协议，如果没有协议，默认会在当前项目中进行查找。下面这种是正常的显示效果 📁 [Quartz Repository](https://github.com/ayuayue/quartz)

### 关于嵌入内容

比如 obsidian 中使用 ![[../../others/services|services]]
这种方式是不支持的，不支持任何的 !\[\[]]，或者使用 # ^ 的写法。

### 加入评论

当前仅支持 utterances 的方式。

1. 在 data/config.yaml 中配置 comment 是否开启，然后配置 utterances 是否开启。
2. 在 layouts/partials/comment.html 中配置仓库的信息，如 
```js
    var utterancesConfig = {
        repo: 'ayuayue/blogtalk',
        issueTerm: 'pathname',
        label: '',
        lightTheme: '',
        darkTheme: ''
    }
```

### 使用 vercel 进行部署并配置域名

