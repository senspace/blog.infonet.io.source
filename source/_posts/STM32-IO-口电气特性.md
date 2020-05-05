---
title: STM32 IO 口电气特性
date: 2019-03-29 14:26:03
updated: 2020-03-21
tags:
- [STM32]
---

# 1. 规格书中内容
**从 STM32F103C8T6 规格书上整理如下表**
在 Excel 里面重新整理了格式，显示图片较小可以放大观看
![IO-static-characteristics](https://file.infonet.io/blog-files/STM32/IO-static-characteristics.png)

# 2. 一般计算值
**当 VDD = 3.3V, VSS = 0V 时，各个参数值如下表**
一般按照 VIL Max = 1.1V, VIH Min = 2.2V 计算就不会有错，准确点的请看下表
![IO-static-characteristics-value](https://file.infonet.io/blog-files/STM32/IO-static-characteristics-value.png)

# 3. 整理的 Excel 文件
- [IO-static-characteristics.xlsx](https://file.infonet.io/blog-files/STM32/IO-static-characteristics.xlsx)
