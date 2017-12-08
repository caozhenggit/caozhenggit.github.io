---
title: Git 配置用户名、密码
date: 2017-07-31 10:40:10
categories: "Git"
---

在终端输入：

``` bash
$ git config --global credential.helper store
```

然后git pull一次，输入一次用户名密码就会自动保存该用户名密码；

查看配置的用户信息：

``` bash
$ git config --list
```