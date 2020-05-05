---
title: SublimeText使用教程
date: 2020-03-21 15:20:14
updated: 2020-03-21 15:20:14
tags:
- [SublimeText]
---

> [Sublime Text](https://www.sublimetext.com/) (3.2.2 版本)

#  1. 汉化

1. 安装 Package Control.
- 方法一：
   1. 打开命令面板: Win/Linux: Ctrl+Shift+P, Mac: Cmd+Shift+P.
   2. 输入 Install Package Control, 然后按 Enter.
- 方法二：（来源：https://packagecontrol.io/installation）
   1. 点击 Preferences -> Browse Packages. （会自动打开软件用户目录。）
   2. Win: 文件件返回上一页 -> 进入 Installed Packages 文件夹。
   3. 下载 Package Control 安装包：
      https://packagecontrol.io/Package%20Control.sublime-package
   4. 将下载的安装包复制到 Installed Packages 文件夹中。
2. 在 Preferences 下就会出现 Package Control.
3. 使用快捷键 Ctrl+Shift+P, 输入 Chinese Localization, 出来后点击安装。

# 2. Package Control 的常用命令

1. Discover Package： 查询插件。（会跳转到Package Control 的官网中。）
2. Install Package ： 安装某个插件。
3. Remove Package： 移除某个已安装的插件。
4. Disable Package ：禁用某个已安装插件。
5. Enable Package ： 启用某个被禁用的插件。
6. List Package ： 列出所有已安装插件。

# 3. 预览 [Markdown](https://www.markdownguide.org/)

1. 打开 Sublime Text -> Preferences(快捷键：Ctrl+Shift+P) -> Install Package
2. 输入 MarkdownLivePreview, 然后按 Enter 安装。
3. 输入 LiveReload, 然后按 Enter 安装。
4. Ctrl+Shift+P, 然后输入 mdp 找到 MarkdownLivePreview: Open Preview 回车就可以打开实时预览。
（传统的本地打开，不支持在线资源的预览）

> 借此分享一个好用的 Markdown 文本编辑器，免费而且还跨平台：[Typora](https://typora.io/)
> 有一定的小缺点就是有多的空行。
