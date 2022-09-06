---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

## 一些命名规范

| 项目命名 | 全部**以小写字母命名，以中划线分割**。如*my-project*。 |
| -------- | ------------------------------------------------------ |
| 目录命名 | 小写字母加下划线，如lib_tomcat。                       |

## 个人备份习惯

```bash
hexo c
git add .
git commit -m "Backup"
git push
hexo g
hexo d
```

## 恢复博客

目前假设本地Hexo博客基础环境已经搭好：比如安装git
、nodejs、hexo安装...

### 克隆项目到本地

输入下列命令克隆博客必须文件

```bash
git clone https://gitee.com/muzihuaner/hexo.git
//https://gitee.com/muzihuaner/hexo.git换成你的
```

### 恢复博客

在clone下来的那个文件夹里面执行

```bash
npm install hexo-cli
npm install
npm install hexo-deployer-git
```

**在此不需要执行hexo init这条指令，因为不是从零搭建起新博客。**

然后就完成了，你如果想也可以

```
hexo clean
hexo g
hexo d
```

