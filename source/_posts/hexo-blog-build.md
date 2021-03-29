---
title: 使用hexo搭建个人博客
categories: 其它
tags:
  - hexo
abbrlink: 43303
date: 2018-11-23 14:35:25
---

### 一、安装Node.js和配置好环境

1. [Node.js](http://nodejs.cn/download/)
2. [Git](http://git-scm.com/)

### 二、使用 npm 即可完成 Hexo 的安装。

```shell
$ npm install hexo-cli -g
```

### 三、快速建站

**初始化blog文件夹，生成相应文件（文件夹名称自定义**

```shell
$ hexo init blog
$ cd blog
```

**启动server（启动成功后在浏览器输入http://localhost:4000 进行预览）**

```shell
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

**创建一个博客文件，名称自定义（source\\_posts\\*.md）**

```shell
$ hexo new "Hello Hexo"
```

More info: [Writing](https://hexo.io/docs/writing.html)

**生成静态网页文件**

```bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

**推送到服务器**

```bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)
