---
title: 搭建个人图床服务器EasyImages2.0
date: 2020-03-21 15:28:48
updated: 2020-03-21 15:28:48
tags:
- [图床]
---

> 项目地址：<https://github.com/icret/EasyImages2.0>
> 推荐使用 [**BT 面板**](https://bt.cn/download/linux.html) 搭建，简单方便。
> 由于我的 VPS 已经搭建了其他网站，为了不影响其他站点，所以我们采用手工搭建。

# 1. 步骤

## 1. 准备好 VPS 和 域名，并安装 PHP7.2 和 Nginx

1. 以 **eg.infonet.io** 为例，以下请将 eg.infonet.io 更新为自己的域名。
2. 详细步骤省略...

## 2. 从 Github 下载 [**EasyImages2.0**](https://github.com/icret/EasyImages2.0) 源码

1. 创建网站目录：

   ```bash
   $ cd /var/www/
   $ mkdir eg.infonet.io
   ```

2. 从 Github 克隆项目：<https://github.com/icret/EasyImages2.0>

   ```bash
   $ cd eg.infonet.io
   $ git clone https://github.com/icret/EasyImages2.0.git
   $ ls
   EasyImages2.0
   ```
   
   <!--more-->

## 3. 配置网站 Nginx

1. 进入到配置文件路径：**/etc/nginx/conf.d/**
2. 新建配置文件 **eg.infonet.io.conf** 内容如下：

   ```markdown
   server {
   
           listen 80;
           listen [::]:80;
   
           server_name eg.infonet.io;
   
           root /var/www/eg.infonet.io/EasyImages2.0;
           index index.php;
           autoindex off;
   
           location / {
                   try_files $uri $uri/ =404;
           }
   
           location ~ \.php$ {
                   include /etc/nginx/fastcgi_params;
                   fastcgi_pass 127.0.0.1:9000;
                   fastcgi_index index.php;
                   fastcgi_param SCRIPT_FILENAME /var/www/eg.infonet.io/EasyImages2.0/$fastcgi_script_name;
           }
   
           location ~ /\.ht {
                   deny all;
           }
   }
   ```

## 4. 检查服务器是否支持本程序

1. 网页打开：<http://eg.infonet.io/check.php>

   ![EasyImage-check.php](https://img.infonet.io/i/2020/03/08/nvylxa.png)

2. 如图可知 GD 库、zip、mbstring 扩展未安装。
3. 安装扩展：

   ```bash
   $ apt install php7.2-gd
   $ apt install php7.2-mbstring
   $ apt install php7.2-zip
   ```

## 5. 设置目录权限

```bash
$ chmod -R 777 /var/www/eg.infonet.io/i/*
```

## 6. 更改图床配置

1. 更改服务器配置文件：/var/www/eg.infonet.io/EasyImages2.0/config.php

# 2. 注意事项

1. 不能开启水印，否则会提示上传失败，但是进入后台查看同一张图片在服务器中会存储 **4** 份。
2. 我的服务器是 512M 内存，当上传图片超过 **1M** 时会上传失败。

> 参考链接：<https://wizardforcel.gitbooks.io/nginx-doc/content/Text/6.3_nginx_ubuntu.html>
