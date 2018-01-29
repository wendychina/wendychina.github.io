---
title: 新建项目没有新建scala类这个选项
date: 2018-01-24 16:40:25
tags: Scala
categories: Scala
---

# 问题描述

IDEA集成环境下，新建一个scala的maven项目，在新项目的src-main-scala目录下右击选择“新建”，结果可以新建的选项里面没有scala类这个选项。差评~

# 原因

这是因为没有设置scala文件夹为source文件夹。

# 解决方法

点击`file-project structure-modules`，将scala文件夹选择为source即可。so easy~
如下图设置一下。
