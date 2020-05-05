---
title: Linux 命令及脚本
date: 2019-03-20 13:35:54
updated: 2020-03-21
tags:
- [Linux]
---

本篇文章记录 Linux 相关的命令及脚本，以便查看备份。
# 1. 修改硬盘名称
```bash
$ e2label /dev/sda2 "name"
```

# 2. Linux 一键安装 SSR
```bash
$ wget -q -N --no-check-certificate https://raw.githubusercontent.com/FunctionClub/SSR-Bash-Python/master/install.sh && bash install.sh
```
脚本是开源的，开源地址：https://github.com/FunctionClub/SSR-Bash-Python

# 3. Linux 一键安装 v2ray
```bash
$ bash <(curl -s -L https://233blog.com/v2ray.sh)
```
脚本是开源的，开源地址：https://github.com/233boy/v2ray

# 4. 查看 Linux 版本内核信息等
```bash
$ uname -a
```

<!--more-->
