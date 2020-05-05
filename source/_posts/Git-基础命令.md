---
title: Git 基础命令
date: 2019-03-26 11:12:40
updated: 2020-03-21
tags:
- [Git]
---

# 1. 基础命令

```bash
$ ssh-keygen (git 生成 ssh 公钥：id_rsa.pub)
$ git config --global user.name "xxx"
$ git config --global user.email "xxx@xxx.xxx"
$ git config --system --list
$ git config --global --list
$ git config --local  --list
$ git add .
$ git status  (查看缓存)
$ git commit -m "xxx"  (commit 备注)
$ git commit --amend  (修改上次 commit 备注信息)
$ git reset --hard
$ git reset --hard HEAD~5
$ git clean -dxf  (清理不是 git 管理得文件)
$ git checkout <file>  (丢弃 file 在工作区得修改)
$ git remote add origin <url>  (添加远程仓库)
$ git push -u origin master  (首次推送至远程仓库)
$ git pull <name>  (从指定远程仓库拉取)
$ git branch  (查看分支)
$ git checkout -b <name>  (创建并切换分支)
$ git merge <name>  (合并某分支到当前分支)
$ git branch -d <name>  (删除分支)
$ git pull = $ git fetch + $ git merge  (从远程拉取)
$ git stash -> $ git stash pop (将当前修改存到暂存区 -> 将之前得修改从暂存区恢复)
$ git submodule update --init --recursive  (拉取目录中其他 git 管理的文件)
$ git config --list
$ git config --global core.editor "vim"
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

<!--more-->

# 2. 常见问题和解决方法

## 1. 分支拷贝

```bash
# example:
#                             H---I---J topicB
#                            /
#                   E---F---G  topicA
#                  /
#     A---B---C---D  master

# command:
$ git rebase --onto master topicA topicB

# result:
#                  H'--I'--J'  topicB
#                 /
#                 | E---F---G  topicA
#                 |/
#     A---B---C---D  master
```

## 2. Fatal: You are not currently on a branch

```bash
$ git checkout -b <new branch>
$ git chechout master
$ git merge <new branch>
$ git branch -d <new branch>
```

## 3. 远程提交比本地新

```bash
# example:
#     o---remotes/origin/master
#     |
#     o---master

# command:
$ git reset --hard origin/master
$ git revert HEAD
$ git push

# result:
#     o---master
#     |
#     o---remotes/origin/master
```

## 4. 修改/合并历史 commit 信息

```bash
$ git rebase -i HEAD~3
$ git commit --amend
$ git rebase --continue
```

## 5. Windows 系统中 Git 文件差异比较工具为 **[Meld](https://meldmerge.org/)**

- 免费
- 跨平台

Meld 安装路径为：**C:/meld/meld.exe**
对应修改的文件为：**.gitconfig**

```bash
$ git config --global merge.tool meld
$ git config --global diff.tool meld
$ git config --global mergetool.meld.path '/c/meld/meld.exe'
```
