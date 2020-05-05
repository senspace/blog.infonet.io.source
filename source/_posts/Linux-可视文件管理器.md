---
title: Linux 可视文件管理器
date: 2019-02-22
updated: 2020-03-21
tags:
- [Linux]
---

# 1. MC 简介
[GNU Midnight Commander](midnight-commander.org) 或者叫 MC 是一个可视文件管理器，根据 GNU 通用公共许可证授权，因此有资格成为自由软件。
它是一个功能丰富的全屏文本模式应用程序，允许您复制，移动和删除文件和整个目录树，搜索文件并在子 shell 中运行命令。 包括内部查看器和编辑器。

# 2. 食用方法

## 步骤
1. 下载地址：http://ftp.midnight-commander.org/mc-4.8.19.tar.xz 
2. 解压后运行：$ ./configure 
3. 运行：$ make 
4. 运行：$ make install

<!--more-->

## 其他说明
1. 其他安装方法请自行 bing/Google 搜索。相信稍微有点 Linux 基础的同学安装不会有任何问题的。
2. 其他版本地址：http://ftp.midnight-commander.org

# 3. 使用教程
1. 使用命令提示符启动 Midnight Commander：$ mc
2. 默认情况下，Midnight Commander 使用蓝色背景颜色并高亮重要的菜单项。可以使用以下指令关闭颜色：$ mc --nocolor
3. 保存常用地址：呼出添加常用地址列表页面，在任意目录列表中，按 **Ctrl+\\** 快捷键,就可以呼出常用文件夹地址列表，在界面中可以按 **Tab** 键定位到下面的操作按钮上。选择目录就可以直接定位到收藏的目录中。
4. 在 mc 界面中，可以使用 **Tab** 切换到左右两边。
5. **Ctrl+o** 切换终端和mc的界面。
6. **Ctrl+u** 切换左右文件列表。
7. **Alt+i** 切换对面的文件列表和光标所在列表一致。
8. **Ctrl+s** 当前界面搜索文件名，默认从文件名开始匹配。
9. **Alt+c** 快速跳转指定目录。
10. 一个使用的小细节：mc 界面中弹出小窗口联系按两下 **Esc** 键可关闭小窗口。

# 4. 参考链接
本文摘自 **[Linux公社](www.linuxidc.com)**，相关连接地址：[Ubuntu 18.04下可视文件管理器Midnight Commander的安装使用](https://www.linuxidc.com/Linux/2019-02/156828.htm)。
