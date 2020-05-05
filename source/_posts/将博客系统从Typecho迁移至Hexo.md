---
title: 将博客系统从Typecho迁移至Hexo
date: 2019-04-22 12:18:43
updated: 2020-03-21
tags:
- [Hexo]
- [Typecho]
---

前段时间在网上看到有个静态博客系统 [Hexo](https://hexo.io)，看了下简介很心动就研究了下。

# 1. 简介

- **[Hexo](https://hexo.io)**

  Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。当然缺点也很明显，就是博客文章多了之后，由博客文件转成 Html 静态文件就变得很慢。
  博客自带的主题配置简单，若采用 [Next](https://theme-next.iissnan.com/) 主题后配置较为复杂，当然功能也就更多。
  主题 Next 教程：[Next](https://theme-next.org/docs/getting-started/) 配置文档。
  Hexo 和 Next 都是开源的，后续有新版本了再升级。
  
  搭建环境：VPS + 域名 + Hexo-v3.8.0 + Next.Gemini-v7.1.0
  ps: Hexo 博客系统也可部署在 [GitHub Pages](https://pages.github.com/) 上, 或者 Coding 上。
  
  *关于 Next 的配置说明： 由于 Next 是开源的，所以不断有新的版本说来，相关的配置最好去从官方文档去查找，不要找网上的一些配置教程，因为有很大的可能是已经过时了。*

<!--more-->

- **[Typecho](http://typecho.org/)**

  Typecho is a PHP Blogging Platform. Simple and Powerful.
  轻量高效. 仅仅7 张数据表，加上不足400KB 的代码，就实现了完整的插件与模板机制。超低的 CPU 和内存使用率，足以发挥主机的最高性能。
  搭建环境：LAMP(or LNMP)
  之前安装的 Typecho 是 v1.1 版本的。

# 2. 总结

Hexo: 每篇文章就是一个 "xxx.md" 文件，方便 Git 管理，后续也方便移植到别的博客系统。
Typecho: PHP + 数据库 形式储存，所有的操作都是在网页端（博客系统的控制台）操作。
当然共同的优点就是简约。

我个人是非常喜欢博客文章以文件的形式储存，可以本地编辑器打开编辑，更直观。不太喜欢基于数据库存储的。
现在是刚开始玩博客的阶段，看到新鲜的事物都想去折腾一番。（笑...）
若后续文章数量达到几百篇的时候，Hexo 生成 Html 静态文件效率没有提高，也会有可能再转去 Typecho。
