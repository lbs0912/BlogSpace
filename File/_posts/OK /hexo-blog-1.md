---
title: 使用GitHub和Hexo搭建免费静态博客(一)
date: 2017-01-17 17:21:00
tags: [Front-end,hexo]
categories: Front-end
---

## 前言
> Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.   --- [Hexo Documents](https://hexo.io/docs/)

本文主要介绍使用GitHub和[Hexo](https://hexo.io/)搭建个人博客的步骤和配置过程。

<!--more-->

本文为该系列博文的第1篇，主要介绍

* `Hexo`的安装
* `GitHub Page`的设置
* 个人域名的申请，DNS的设置
* Hexo博客主题的设置
* 博客的撰写和发表

## 更新历史
* 博文发表 - 2016-11-12
* Next主题更换 - 2017-01-08
* 博文排版更新 - 2017-02-06

## 安装所需项
### 安装Git
参考 [How to install github](https://desktop.github.com/)完成GitHub的安装与基础配置。
### 安装Node.js
* 参考Node.js官方网站[Node.js](https://nodejs.org/en/)完成Node.js的安装与基础配置
* 参考[Node.js安装和NPM路径修改](http://liubaoshuai.com/2016/09/25/node-install/)完成相关配置

###  安装Hexo
> [Hexo](https://hexo.io/) is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.  ----  [Hexo](https://hexo.io/)

参考Hexo官方网站[Hexo](https://hexo.io/)，了解Hexo更多信息，并完成Hexo的安装。

```
npm install -g hexo-cli
```
##  Hexo初始化配置
### 初始化Hexo
创建Hexo目录（此处以`E:\MyBlogRepository`目录为例）用于存放博客文档，并进行Hexo初始。
```cmake
mkdir MyBlogRepository
cd MyBlogRepository
hexo init    #hexo初始化
npm install  #npm将自动安装所需的组件
hexo -v      #查看hexo版本，验证是否安装成功
```
Hexo安装完成之后，其目录如下。
* `.deploy` :  需要部署的文件
* `node_modules`:  Hexo插件
* `public`:  生成的静态网页文件
* `scaffolds`:  模板
* `source`: 博客正文和其他源文件，404，favicon，CNAME
   - `_drafts`:  草稿
   - `_posts`:  文章
* `themes`:  主题
* `_config.yml`:  全局配置文件
* `package.json`

### 本地查看效果
```cmake
hexo server   
```
执行上述命令，并访问`http://localhost:4000/`可以查看Hexo页面效果。按下`Ctrl`+`C`可以退出hexo server。
###  安装Hexo插件
安装Hexo插件来增强Hexo效果和美化页面。
```cmake
npm install hexo --save
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save #deploy to git
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked --save
npm install hexo-renderer-stylus --save
npm install hexo-generator-feed --save   # RSS
npm install hexo-generator-sitemap --save #sitemap
```
###  Hexo常用命令(Hexo博客操作命令行)
``` cmake
hexo new "My New Post"  # create a new post
hexo server    # run hexo server
hexo generate  # Generate static files
hexo deploy   # Deploy to remote sites
hexo n   #hexo new的简写
hexo s   # hexo server的简写
hexo g   # hexo generate的简写
hexo d   # hexo deploy的简写
hexo clean  # 清楚缓存文件
hexo -v     # 查看hexo版本
hexo help   # 查看hexo帮助
hexo d -g   # 生成部署  组合命令
hexo s -g   # 生成预览  组合命令
hexo s --debug  # 本地预览，并开启调试模式
```

## 部署静态网页到GitHub
### 创建GitHub仓库
* 注册GitHub账号并登录（此处以`lbs0912`为例）。
* Create a new repository and name it as `YourAccountName.github.io`（此处以`lbs0912.github.io`为例）。
* 在本库的设置界面中，选择GitHub Page的主题，完成之后可以访问`https://lbs0912.github.io/`进行预览。

###  同步内容至GitHub
在Hexo安装目录下（此处以`E:\MyBlogRepository`目录为例）打开`_config.yml`文件，进行如下修改。
```cmake
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/lbs0912/lbs0912.github.io.git
  branch: master
```
需要注意的是，该配置文件遵循`Yaml`语法，`type:`和`git`中间需有一空格。

对静态网页的标题，子标题，介绍，时区等内容进行如下设置。
```cmake
# Site
title: Liu Baoshuai's Blog
subtitle: Do one thing at a time and do well.  
description: Record and become better myself.
author: Liu Baoshuai
language: zh-Hans
timezone: Asia/Shanghai

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://lbs0912.github.io/
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
```
配置文件修改完成后，输入如下命令，将更新后的内容同步至GitHub。打开`https://lbs0912.github.io/`可以访问博客界面。
``` cmake
hexo clean      
hexo generate    # or hexo g
hexo deploy      # or hexo d
```

后续每次更新博客时候，都要输入上述命令。也可以输入简化命令`hexo d -g`。

## 域名和DNS
### 域名申请
可以申请自己的域名并用于该博客的访问。此处申请域名`liubaoshuai.com/`。

### 设置CNAME
在Hexo的安装目录下的`source`目录下，创建`CNAME`文件，并存入申请的域名`liubaoshuai.com`。

### DNS
推荐使用[DNSPod](https://www.dnspod.cn/)进行DNS解析。此处，由于域名`http://liubaoshuai.com/`在阿里云购买，故使用阿里云的DNS云解析。DNS界面进行如下设置。
| 记录类型 |   主机记录 | 记录值      |
| :--------:  | :--------:| :--:  |
| CNAME | www | lbs0912.github.io |
| A     |  @  |  192.30.252.154   |
| A     | @   | 192.30.252.153    |
其中A记录为GitHub Page提供的IP地址，可以访问[GitHub Page](https://help.github.com/articles/github-s-ip-addresses/)查询最新IP地址。

最后，执行如下命令，并访问`liubaoshuai.com`查看修改效果。
``` cmake
hexo clean  
hexo g    # or hexo generate
hexo d    # or hexo deploy
```

## 访问博客
至此，便可通过访问自己的域名来访问自己的博客。
## 后续介绍
关于Hexo参数配置和Hexo的进阶使用，参考该系列博文后续介绍。
## 参考资料
[1] [使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
[2] [Hexo在github上构建免费的Web应用](http://blog.fens.me/hexo-blog-github/)
[3] [使用hexo+github搭建静态博客](https://qiutc.me/post/%E4%BD%BF%E7%94%A8hexo-github%E6%90%AD%E5%BB%BA%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2.html)
[4] [Hexo博客系列](http://www.isetsuna.com/hexo/writing-image/)
[5] [使用Hexo基于GitHub-Pages搭建个人博客](http://ehlxr.me/2016/08/30/%E4%BD%BF%E7%94%A8Hexo%E5%9F%BA%E4%BA%8EGitHub-Pages%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%B8%89%EF%BC%89/)
[6] [使用Hexo基于GitHub-Pages搭建个人博客（三）](http://ehlxr.me/2016/08/30/%E4%BD%BF%E7%94%A8Hexo%E5%9F%BA%E4%BA%8EGitHub-Pages%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%B8%89%EF%BC%89/)
[7] [Hexo博客搭建](https://neveryu.github.io/2016/09/03/hexo-next-one/)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 