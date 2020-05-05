---
title: 部署Hexo
date: 2020-03-21 15:18:42
updated: 2020-03-21 15:18:42
tags:
- [Hexo]
---

> **Linux 用户**

# 1. 安装 [Git](https://git-scm.com/)

> 此处省略。

# 2. 安装 [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

> **这里推荐从 [nvm](https://github.com/nvm-sh/nvm) 安装。**

1. 安装或者更新 nvm(v0.35.2):

   ```bash
   $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
   # 或者
   $ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
   ```

2. 修改环境变量：

   ```bash
   # 第一步：
   $ export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
   # 第二步：
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"    # This loads nvm
   ```

3. 验证是否已经安装 nvm 输入：

   ```bash
   $ command -v nvm
   nvm
   # 若输出 nvm 代表安装成功。请注意 which nvm 将不起作用。Since nvm is a sourced shell function, not an executable binary.
   ```

4. 安装 Node.js 和 npm

   ```bash
   $ nvm install node
   # 安装 Node.js 时会一起安装 npm.
   ```

5. 更多请查看 nvm GitHub 主页。

   https://github.com/nvm-sh/nvm

# 3. 安装 [Hexo](https://hexo.io/)

```bash
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

# 4. 使用 [Hexo](https://hexo.io/)

```bash
$ hexo new [layout] <title>    # 新建一篇文章
$ hexo clean && hexo g && hexo s    # 生成静态文件并启动本地服务器
$ hexo clean && hexo g && hexo d    # 生成静态文件并本地部署
```

> 更多请看 Hexo 基础命令章节或者请看官网教程: https://hexo.io/
