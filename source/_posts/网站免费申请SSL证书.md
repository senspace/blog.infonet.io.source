---
title: 网站免费申请SSL证书
date: 2020-03-21 15:30:21
updated: 2020-03-21 15:30:21
tags:
- [SSL]
---

> 以下用 **Certbot+Nginx+Ubuntu18.04** 为例：

# 1. 简单总结

```bash
# 下一次继续添加证书的时候就用如下命令：

# 1. 自动运行
$ sudo certbot --nginx

# 2. 选择对应的网址。
Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: infonet.io
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1

# 3. 请选择 2: 重定向。
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2

# 4. 出现如下信息表示 SSL 证书配置成功。
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://infonet.io
```

<!--more-->

# 2. 详细讲解

## 1. SSH into the server

SSH into the server running your HTTP website as a user with sudo privileges.

## 2. Add Certbot PPA

You'll need to add the Certbot PPA to your list of repositories. To do so, run the following commands on the command line on the machine:

   ```bash
   $ sudo apt-get update
   $ sudo apt-get install software-properties-common
   $ sudo add-apt-repository universe
   $ sudo add-apt-repository ppa:certbot/certbot
   $ sudo apt-get update
   ```

## 3. Install Certbot

Run this command on the command line on the machine to install Certbot.

```bash
$ sudo apt-get install certbot python-certbot-nginx
```

## 4. Choose how you'd like to run Certbot

- Either get and install your certificates...

  Run this command to get a certificate and have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step.

  ```bash
  $ sudo certbot --nginx
  ```

- Or, just get a certificate

  If you're feeling more conservative and would like to make the changes to your Nginx configuration by hand, run this command.

  ```bash
  $ sudo certbot certonly --nginx
  ```

## 5. Test automatic renewal

The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command:

```bash
$ sudo certbot renew --dry-run
```

The command to renew certbot is installed in one of the following locations:

- `/etc/crontab/`
- `/etc/cron.*/*`
- `systemctl list-timers`

## 6. Confirm that Certbot worked

To confirm that your site is set up properly, visit `https://yourwebsite.com/` in your browser and look for the lock icon in the URL bar. If you want to check that you have the top-of-the-line installation, you can head to https://www.ssllabs.com/ssltest/.

> 参考链接：
> 1. Certbot：https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx
> 2. 重定向：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Redirections
