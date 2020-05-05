---
title: 如何制作Win10系统盘或安装盘
date: 2019-05-01 00:01:40
updated: 2020-03-21
tags:
- [Windows]
---

# 1. 准备
- 写盘软件：
   *此教程需要使用一次，为此我们免费下载试用，若要经常使用请购买正版软件。*
   **[点击此处免费下载：免费下载UltraISO软碟通官方中文版](https://cn.ultraiso.net/xiazai.html)**

- Windows10 系统：
   建议在下面的网址中按需选择相应的系统，这里演示采用 Windows10 系统。
   **[点击此处打开网页下载 Windows10: MSDN, 我告诉你](http://msdn.itellyou.cn/)**
   我也比较推荐大家都用 Windows10 系统，因为驱动相对完整一些。

<!--more-->

![i-tell-you-win10](https://file.infonet.io/blog-files/Win10/i-tell-you-win10.png)

# 2. 写盘
- 打开要写入到 U 盘的 ISO 文件：文件 -> 打开 -> 选择应对的 ISO 文件(本次用 xubuntu 代替)
- 写入 U 盘：启动 -> 写入硬盘映像 -> 选择对应的**硬盘驱动器** -> (格式化+)写入 -> 等待完成
![UltraISO](https://file.infonet.io/blog-files/Win10/UltraISO.png)

# 3. 其他
- 此款软件也可以写入其他 ISO 文件到 U 盘或者硬盘。
- Windows 系统的安装盘通过这种方法制作相对较好，之前测试过在 Linux 系统下通过 **dd** 命令到 U 盘，结果不能启动。
- Linux 用户可以通过 **dd** 命令来写入非 Windows ISO 文件。

