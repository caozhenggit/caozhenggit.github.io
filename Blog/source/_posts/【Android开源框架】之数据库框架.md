---
title: 【Android开源框架】之数据库框架
date: 2017-09-27 12:40:27
categories: "Android"
tags: "数据库"
---

## [GreenDAO](https://github.com/greenrobot/greenDAO) ##

GreenDAO是一个轻量级，快速的orm框架。简化建表、查询、更新、插入、事务、索引的操作
特性:

- 性能突出(比ormlite快4-5倍), performance
- 库小，核心包小于100k
- 简单易用的API
- 支持protobuf
- 自动生成数据库访问代码

## [Realm](https://github.com/chennaione/sugar) ##
移动端的数据库，适用于 Phone、Tablet、Wearable，支持 ORM，线程安全、支持连表及数据库加密，比 SQLite 性能更好。
特性:

- 着重移动端
- 简单易用的API
- 支持线程安全，关系数据库和加密
- 访问快速
- 跨平台

## [OrmLite](https://sourceforge.net/projects/ormlite/files/releases/com/j256/ormlite/) ##
OrmLite不是Android平台专用的orm框架，它是一个Java orm，OrmLite For Android增加了对Android平台的支持。

## [ActiveAndroid](https://github.com/pardom/ActiveAndroid)  ##
ActiveAndroid是一个轻量级的orm框架，名称命令方式类似于Yii、Rails等使用的orm框架ActiveRecord

## [DBFlow](https://github.com/Raizlabs/DBFlow)  ##
一个速度极快，功能强大，而且非常简单的 Android 数据库 ORM 库

## [Sugar](https://github.com/chennaione/sugar)  ##
用超级简单的方法处理Android数据库
特性:

- 配置少
- 自动生成表结构
- 支持在不同模式版本直接切换