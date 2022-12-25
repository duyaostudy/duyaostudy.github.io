---
title: hexo + github搭建个人博客
tags: [github, hexo, blog]
categories: blog
date: 2022-12-16 10:42:00
toc: true
thumbnail: /2022/12/16/hexo-github/banner.png
---
# 前置需要

* git
* node.js -v16.15.1
* github下一个username.github.io仓库

**git下载**
windows: [git](https://git-scm.com/download/win)

**node.js**
官方平台安装：[Node.js](https://nodejs.org/en/download/)

> 版本仅供参考

<!--more-->

# 搭建流程

**hexo官方文档**

[官方文档 | Hexo](https://hexo.io/zh-cn/docs/)

**安装hexo**

使用npm安装hexo

```
$ npm install -g hexo-cli
```

**初始化hexo**

在你创建的文档里

```
$ hexo init <folder>
$ npm install
```

> folder 为生成在那个文档路径下，默认为当前文档

**生成的文档结构**

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

**_config.yml文件**

主要配置文件，网站的大部分配置参数在此设置

**source**

资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，**而其他文件会被拷贝过去**。

**source/_posts**

存放post布局格式的笔记，存入.md格式的文件，hexo会根据hexo生成静态界面，可在scaffolds/文档下修改格式文件

# _config.yml 配置属性

**只列举经常一些初始的，其他看官方文档**

title: 网站标题
subtitle: 网站副标题
description: 描述
keywords: 网站关键字
author: 你的名字
language: 使用的语言
timezone: 时区

> url: http://username.github.io
>
> url改为你的github名称地址，如果有子目录在地址后添加子目录名称

theme: 主题名

# 生成部署

**generate**

```
$ hexo generate
```

生成静态文件，可以简写为

```
$ hexo g
```

**server**

```
$ hexo server
```

启动本地服务器查看效果

默认为4000端口

简写

```
$ hexo s
```

**deploy一键部署**

部署网站
安装插件

```
$ npm install hexo-deployer-git --save
```

> 使用前请配置自己的git

**在config.yml下添加**

```yml
deploy:
  type: git
  repo: git库的名称 #https://github.com/username/username.github.io
  branch: 要上传的分支名称
  message: 自定义提交信息
```

生成站点文件并推送至远程库
执行 hexo clean && hexo deploy

**push部署**

可以将生成文件下的public文件直接push到自己仓库，也可以访问

> **访问https://username.github.io就可以查看自己的blog了**

**主题**

可以到hexo官网主题商店找到自己喜好的主题，相关配置可在主题页中找到

个人使用主题：[icarus](https://github.com/ppoffice/hexo-theme-icarus)

# 常见问题

* github网络问题导致push不到仓库

# 搭建建议

* 在github上创建hexo分支设为默认分支，用于存放hexo相关的源代码
  * 可在别处拉取代码更新自己的blog
* main分支用于存放生成后public资源
