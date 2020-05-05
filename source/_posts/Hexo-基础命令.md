---
title: Hexo 基础命令
date: 2019-09-22 13:38:50
updated: 2020-03-21
tags:
- [Hexo]
---

# 1. 常用命令
```markdown
$ hexo new <title>
$ hexo clean && hexo g && hexo s
$ hexo clean && hexo g && hexo d
```

<!---more--->

# 2. 详解

## 1. 新建一篇文章

新建一篇文章,如果没有设置 layout,默认使用 _config.yml 中的 default_layout参数代替。
如果 title 中有空格，请使用引号括起来。

```markdown
$ hexo new [layout] <title>
```

|   |   |
|:-:|:-:|
| 参数 | 描述 |
| -p, --path   | 自定义新文章的路径                          |
|-r, --replace | 如果存在同名文章，将其替换                   |
|-s, --slug    | 文章的 Slug，作为新文章的文件名和发布后的 URL |

## 2. 生成静态文件

```markdown
$ hexo genrate
$ hexo g
```

## 3. 启动服务器

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

```markdown
$ hexo server
$ hexo s
```

## 4. 部署网站

```markdown
$ hexo deploy
$ hexo d
```

|                |                       |
|:--------------:|:---------------------:|
| 参数           | 描述                   |
| -g, --generate | 部署之前预先生成静态文件 |

## 5. 清楚缓存文件

```markdown
$ hexo clean
```
