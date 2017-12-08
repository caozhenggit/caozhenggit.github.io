---
title: Git 分支管理
date: 2017-12-08 10:17:42
categories: "Git"
tags: "git"
---

### 创建本地分支 ###
git branch <分支名>

### 切换分支 ###
git checkout <分支名>

### 推送本地分支关联远程分支 ###

- 远程分支存在并且已关联本地分支：	git push
- 远程分支存在且本地分支也存在，但未关联：git push -u origin <远程分支名>
- 远程分支不存在但本地分支存在：git push origin <本地分支名>:<远程分支名>

### 拉取远程分支并创建本地分支 ###
git checkout -b <本地分支名> origin/<远程分支名>

### 重命名分支 ###
 git branch -m | -M <旧分支名> <新分支名>

### 删除本地分支 ###
 git branch -d <分支名>


### 删除远程分支 ###
 git branch -d -r <分支名>

### 查看本地分支 ###
 git branch

### 查看本地与远程分支 ###
 git branch -a