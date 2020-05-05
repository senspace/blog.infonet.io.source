---
title: Hexo使用总结
date: 2020-03-24 19:22:31
updated: 2020-04-19 14:50
tags:
- [Hexo]
---

# 1. 开启站点地图

```bash
# 开启站点地图，需要安装 hexo-generator-sitemap 扩展包
npm install hexo-generator-sitemap --save
```

# 2. 开启本地搜索

```bash
# 开启本地搜索，需要安装 hexo-generator-searchdb 扩展包
npm install hexo-generator-searchdb
```

# 3. 部署到服务器

```bash
# 部署到服务器，需要安装 npm install hexo-deployer-git --save 扩展包
npm install hexo-deployer-git --save
```

# 4. 查看 Hexo 运行所需要的依赖

```bash
npm ls --depth 0
# 如果还想展开一层看看这些模块各自又依赖了哪些模块，那就运行下面的命令
npm ls --depth 1
```
