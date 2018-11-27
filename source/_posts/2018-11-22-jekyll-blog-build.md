---
layout: post
title: 使用Jekyll + GitHub Pages搭建个人博客
date: 2018-11-22
categories: 其它
tags: [Jekyll]
---

### 前言

一直就想搭建一个属于自己的博客网站，但是一直拖着没有执行，在一次偶然的机会看到了鸿洋大神的 <a href="http://blog.csdn.net/lmj623565791/article/details/51319147" target="_blank">如何利用github打造博客专属域名</a>，就心血来潮，马上自己动手做了一个，耗时了近一个星期，终于差不多完成了，特意记录下来，供他人参考。

### 目录

- [关于Jekyll](#about-jekyll)

- [关于GitHub Pages](#about-github_pages)

- [本地环境搭建](#local-environment)

  - [安装Ruby](#install-ruby)

  - [获取博客模板](#clone-blog)

  - [安装Bundler](#install-bundler)

- [创建自己的GitHub Pages](#create-myblog)

- [修改模板，上传至服务器](#push-to-server)

- [安装异常问题处理](#install-error)

- [编写文章](#writing)

- [参考资源](#reference-data)

#### <a name="about-jekyll"></a>关于Jekyll

　Jekyll 是一个免费的生成静态网页的工具，不需要数据库支持。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，可以配合第三方服务例如： Disqus（评论）、多说(评论) 以及分享 等等扩展功能，Jekyll 可以直接部署在 Github（国外） 或 Coding（国内） 上，可以绑定自己的域名。<a href="http://jekyll.bootcss.com/" target="_blank">Jekyll中文文档</a>、<a href="https://jekyllrb.com/" target="_blank">Jekyll英文文档</a>、<a href="http://jekyllthemes.org/" target="_blank"></a>。

#### <a name="about-github_pages"></a>关于GitHub Pages

　官方说法是Websites for you and your projects.<a href="https://pages.github.com/" target="_blank"></a>是一个免费的静态网站托管平台，由github提供，它具有以下特点：

1. GitHub Pages 有 300M 免费空间，资料自己管理，保存可靠；

2. 学着用 GitHub，享受 GitHub 的便利，上面有很多大牛，眼界会开阔很多；

3. 顺便看看 GitHub 工作原理，最好的团队协作流程；

4. GitHub 是趋势；

5. 可以自定义域名

#### <a name="local-environment"></a>本地环境搭建

如果你只是想使用本主题，而不想搭建本地环境，那么可以直接跳过这部分，不搭建本地环境则不能实现本地预览，以下安装操作都是在Windows系统环境下进行。

##### <a name="install-ruby"></a>安装Ruby

jekyll本身基于Ruby开发，因此，想要在本地构建一个测试环境需要具有Ruby的开发和运行环境。在windows下，可以使用<a href="http://rubyinstaller.org/downloads/" target="_blank">Rubyinstaller</a>安装（选择带有DEVKIT的安装包）<a href="http://www.ruby-lang.org/zh_cn/downloads/" target="_blank"></a>，Windows下只需要保持默认状态一路下一步就可以了。

##### <a name="clone-blog"></a>获取博客模板

$ git clone https://github.com/JasonRH/Blog-Jekyll.git

##### <a name="install-bundler"></a>安装Bundler

建议使用<a href="http://bundler.io/" target="_blank"></a>来安装和运行Jekyll。

进入仓库中，直接使用下面命令即可：

`$ gem install bundler `

`$ bundle install`
 命令会根据当前目录下的Gemfile（当无法下载时可将source源修改），安装所需要的所有软件。这一步所安装的东西，可以说跟github本身的环境是完全一致的，所以可以确保本地如果没有错误，上传后也不会有错误。而且可以在将来使用下面命令，随时更新环境，十分方便

`$ bundle update`
 使用下面命令，启动转化和本地服务；（每次重新clone下仓库后都需要update）

`$ bundle exec jekyll server`
 在浏览器里输入： <a href="http://localhost:4000" target="_blank">http://localhost:4000</a>，就可以看到博客效果了。

##### <a name="create-myblog"></a>创建自己的GitHub Pages

在你的github里创建一个username.github.io的仓库，username指的是你的github的用户名。

##### <a name="push-to-server"></a>修改模板，上传至服务器

将获取的模板复制，放到你自己的仓库中。

在仓库中进行以下操作：

1.把 _posts/ 目录下的文章都去掉。

2.修改 _config.yml 文件里面的内容为你自己的。

3.本地预览: 进入仓库 $ bundle update，然后在浏览器输入：http://localhost:4000

4.确认修改完成后直接push到服务器即可。

然后你在浏览器里输入 username.github.io ，就可以访问你自己的博客了。

##### <a name="install-error"></a>安装异常问题处理

![](/picture/2018-11-22.png)

解决方案：

在ruby的安装目录下找到engine.rb文件，目录格式如`C:\Ruby25-x64\lib\ruby\gems\2.5.0\gems\sass-3.7.2\lib\sass在文件中添加一行`Encoding.default_external = Encoding.find('utf-8')  
`在require语句结束处，如：

```
require 'sass/media'
require 'sass/supports'
module Sass   
Encoding.default_external = Encoding.find('utf-8')
```

目录结构

　Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径, 你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

一个基本的 Jekyll 网站的目录结构一般是像这样的：

```
.
├── _config.yml  
├── _includes  
|   ├── footer.html  
|   └── header.html  
├── _layouts  
|   ├── default.html  
|   ├── post.html  
|   └── page.html  
├── _posts  
|   └── 2017-08-01-welcome-to-jekyll.markdown  
├── _sass  
|   ├── _base.scss  
|   ├── _layout.scss  
|   └── _syntax-highlighting.scss  
├── about.md  
├── css  
|   └── main.scss  
├── feed.xml  
└── index.html
```

这些目录结构以及具体的作用可以参考 <a href="http://jekyll.com.cn/docs/structure/" target="_blank"></a>

进入 _config.yml 里面，修改成你想看到的信息，重新 jekyll server ，刷新浏览器就可以看到你刚刚修改的信息了。

到此，博客初步搭建算是完成了，

### <a name="writing"></a>编写文章

　　所有的文章都是 _posts 目录下面，文章格式为 mardown 格式，文章文件名可以是 .mardown 或者 .md。

　　编写一篇新文章很简单，你可以直接从 _posts/ 目录下复制一份出来 `2017-08-01-welcome-to-myblog.md` ，修改名字为 2017-08-01-article1.markdown ，注意：文章名的格式前面必须为 2017-08-01- ，日期可以修改，但必须为 年-月-日- 格式，后面的 article1 是整个文章的连接 URL，如果文章名为中文，那么文章的连接URL就会变成这样的：%E6%90%AD%E5， 所以建议文章名最好是英文的或者阿拉伯数字。 双击 2017-08-01-article1.markdown 打开

### <a name="reference-data"></a>参考资源

- <a href="http://baixin.io/2016/10/jekyll_tutorials1/" target="_blank">潘柏信-Jekyll搭建个人博客</a>

- <a href="https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/" target="_blank">GitHub Pages官方文档</a>

- <a href="http://www.pchou.info/ssgithubPage/2014-07-04-build-github-blog-page-08.html" target="_blank">一步步在GitHub上创建博客主页</a>
