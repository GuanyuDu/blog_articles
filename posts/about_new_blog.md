---
title: "关于为什么要重构博客这件事"
date: 2022-03-10 02:03:30
weight: 1
tags: ["Life"]
author: "Guanyu"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
canonicalURL: "https://canonical.url/to/page"
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---


## 伊始

一直觉得博客是个很酷的东西，作为开发者怎么能不拥有一个呢？虽然也有一些现有的博客平台，但是感觉不够高级（不够折腾）。于是把精力花在了挑选博客框架，选择好看的博客主题，始终只是流于表面。

开始玩博客的心态基本是这样的：

#### Day one ...

「博文、评论、友链、图片墙、树洞我全都要。」

#### Day two ...

「欸嘿，看板娘真可爱，加上加上！」

#### One year later...

「水文 +1，水文+++。」


反观大佬们的博客，各个都是朴实无华，干货为主，比如这些：

[阮一峰的网络日志](https://www.ruanyifeng.com/blog/)

[张鑫旭-鑫空间-鑫生活](https://www.zhangxinxu.com/wordpress/)

[Spencer Woo](https://spencerwoo.com/blog) 

> PS：别问我为什么关注这么多前端大佬，前端实在是太好玩了🤣


## 当下

从技术上来说，之前使用的是 [Hexo](https://hexo.io/zh-cn/) ，它分为博客、管理后台两套页面，博客页面基于模板引擎实现，而管理后台是一个单页应用。这套框架的后台是用 Java 开发的，当初选它也是因为这点，熟悉的语言改动起来毕竟更简单。相对 [Hugo](https://gohugo.io/) 来说功能比较丰富，整套后台除了发布博文，还包含了 CMS 的功能省去了搭建图床的烦恼，代价就是服务更重一些。

现在更换成了由 Go 开发的 [Hugo](https://gohugo.io/) 框架，通过将 Markdown 文件转换成 HTML 页面的方式去构建博客，非常的轻量。
博客样式则是基于开源项目 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题修改而来。将原本的英文字体 Ubuntu 更换成了 Overpass，中文更换成了无衬线字体 sans-serif，再做了一些字号、圆角等小的样式修改。


更换框架只是顺带的，这次改版的重点主要放在了文章的质量以及博客的阅读体验上。第一点自然不用说，而阅读体验比较特殊，现在中文互联网上的博客、博文很多，但是要说阅读体验大部分都很糟糕，稀烂的排版，纯文本复读机。

写代码有规范，写文章自然也有，其实前人已经研究出了很多非常好的写作规范，比如我一直在使用的《[中文排版指北](https://github.com/mzlogin/chinese-copywriting-guidelines/blob/Simplified/README.md)》就很推荐大家学习，当时是在 [少数派](https://sspai.com/) 编辑部了解到这套规范，事实上 Apple、Microsoft 都在使用这套编写规范。

作为一个在设计上有着追求的开发者，自然不能随波逐流，大部分的样式改动都是在考虑「如何能给读者带来更佳的阅读体验」，这样的思考会伴随着博客一直存在。

这次改版只保留了博文功能，其他的全部砍掉了，后续可能会考虑添加评论功能，毕竟和读者互动也是很重要的一部分。


## 未来

目前不管是笔记还是博文都是用 Typaro 写在本地（话说 Typaro 也开始收费了 💔），再使用 Git 进行同步，但终究不是很方便。

看到 Notion 最近推出了针对个人用户的 API 接口，已经有大佬实现了通过 Notion API 获取文章 Block 再转换成对应的静态 HTML 博文，具体可以看 
[Powering my blog with Notion](https://spencerwoo.com/blog/nextjs-blog-notion) 和 
[Revisiting blogging with Notion in 2022](https://spencerwoo.com/blog/revisiting-blogging-with-notion-2022) 这两篇文章。

未来可能也换成这种模式。

