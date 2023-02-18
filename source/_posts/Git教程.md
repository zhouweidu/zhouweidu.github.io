---
title: Git教程
date: 2022-09-06 19:07:10
tags:
- Git
categories:
- Git
typora-root-url: Git教程
---

# Git教程

## 1. 一些常用的git命令

![image-20220505162534378](image-20220505162534378.png)

- ctrl+L清空屏幕（输入clear然后回车也可以）
- git reflog 查看版本信息
- git log 查看版本详细信息

![image-20220505171148921](image-20220505171148921.png)

下面是一个很好用的查看提交记录的命令，可以可视化的查看commit和merge

```txt 
git log --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative 
```

```txt
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%
d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```

## 2. 廖雪峰的git教程

[多人协作git - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320)

![image-20221101100754052](image-20221101100754052.png)
