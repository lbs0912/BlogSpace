
# Blog 搭建和配置

* [Changelog](#Changelog)
* [Ref](#Ref)
  * [Hexo 基础使用](#Hexo-基础使用)
  * [Blog 进阶管理](#Blog-进阶管理)
  * [优质博客参考](#优质博客参考)
* [Hexo](#Hexo)
  * [Install](#Install)
  * [命令行](#命令行)
* [Hexo to Github](#Hexo-to-Github)
  * [创建GitHub仓库](#创建GitHub仓库)
  * [同步内容至 GitHub](#同步内容至-GitHub)
* [域名和DNS](#域名和DNS)
  * [域名](#域名)
  * [设置CNAME](#设置CNAME)
  * [DNS](#DNS)
* [Blog 配置](#Blog-配置)
  * [Plugin](#Plugin)
  * [添加Meta信息](#添加Meta信息)
  * [分页功能](#分页功能)
  * [Hexo 主题配置](#Hexo-主题配置)
  * [集成第三方服务](#集成第三方服务)
  * [404页面](#404页面)
  * [Fork me on GitHub](#Fork-me-on-GitHub)
  * [背景音乐播放设置](#背景音乐播放设置)
  * [为 Blog 添加 README](#为-Blog-添加-README)
  * [背景效果优化](#背景效果优化)
  * [博文置顶](#博文置顶)
  * [博文收起添加](#博文收起添加)
  * [文章加密阅读](#文章加密阅读)
  * [定制CSS](#定制CSS)
  * [主页图片展示](#主页图片展示)
  * [博文底部标签样式](#博文底部标签样式)
  * [首页-简历和相册分类](#首页-简历和相册分类)
* [最后](#最后)


## Changelog
* 2018/08/23，撰写
* 2018/09/04，整理
* 2018/09/25，添加 `Font Awesome`使用
* 2019/03/18，添加文章加密阅读
* 本博客全部配置信息可在 [HexoBlog | lbs0912-github](https://github.com/lbs0912/HexoBlog) 查看

## Ref

### Hexo 基础使用
* [使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
* [使用Hexo基于GitHub-Pages搭建个人博客（三）](http://ehlxr.me/2016/08/30/%E4%BD%BF%E7%94%A8Hexo%E5%9F%BA%E4%BA%8EGitHub-Pages%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%B8%89%EF%BC%89/)

### Blog 进阶管理
* [Next Theme 官方配置手册](https://theme-next.iissnan.com/)
* [如何更好地对hexo博客管理](http://feg.netease.com/archives/634.html)
* [Hexo 博客进阶配置](http://blog.junyu.io/posts/0010-hexo-learn-from-Never-yu.html#background)
* [Hexo搭建的GitHub博客之优化大全](https://zhuanlan.zhihu.com/p/33616481)
* [Next Theme 相册配置](https://timding.top/2017/09/18/Hexo-NexT-%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E7%9B%B8%E5%86%8C-%E4%BA%8C/) 
* [搭建Hexo 相册](https://juejin.im/post/5b8bc953518825284910dcdd)

### 优质博客参考
* [Sunmengyuan Blog](https://sunmengyuan.github.io/garden/)
* [羡辙 Blog](http://zhangwenli.com/)
* [BYvoid](https://www.byvoid.com/)



## Hexo

### Install

> Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.   --- [Hexo](https://hexo.io/docs/)

 
参考 [Hexo官网](https://hexo.io/)了解Hexo更多信息。


```
npm install hexo-cli -g
mkdir Blog && cd Blog

hexo init blog
cd blog
npm install
hexo server
```

Hexo生成的目录结构如下
* `.deploy` :  需要部署的文件
* `node_modules`
* `public`:  生成的静态网页文件
* `scaffolds`:  模板
* `source`: 博客正文和其他源文件，404，favicon，CNAME
   - `_drafts`:  草稿
   - `_posts`:  文章
* `themes`:  主题
* `_config.yml`:  全局配置文件
* `package.json`

###  命令行

``` bash
hexo new page archive
# 创建分类目录并初始化index.md  等同于  hexo n

hexo server    
# run hexo server  等同于  hexo s

hexo generate  
# Generate static files   等同于  hexo g

hexo deploy   
# Deploy to remote sites   等同于  hexo d

hexo clean  # 清空缓存文件
hexo -v     # 查看hexo版本
hexo help   # 查看hexo帮助
hexo d -g   # 生成部署  组合命令
hexo s -g   # 生成预览  组合命令
hexo s --debug  # 本地预览，并开启调试模式
```

在后续博文发布时，依次执行如下命令
* `hexo clean`: 清空缓存文件
* `hexo g`: 编译产生静态文件
* `hexo s`: 本地预览，可选
* `hexo d`: 部署到服务端

## Hexo to GitHub

### 创建GitHub仓库

* 创建一个仓库，并命名为 `YourAccountName.github.io`（此处以`lbs0912.github.io`为例）
* 设置仓库属性，选择 `GitHub Page` 的主题，访问 `https://lbs0912.github.io/`进行预览

###  同步内容至 GitHub

在Hexo安装目录下打开 `_config.yml` 文件，进行如下修改

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/lbs0912/lbs0912.github.io.git
  branch: master
```
 
> 该配置文件遵循 `Yaml` 语法，`type:` 和 `git` 中间需有一空格。

对静态网页的标题，子标题，介绍，时区等内容进行如下设置。

```
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

//...

highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
```

配置文件修改完成后，输入如下命令，将更新后的内容同步至GitHub。

``` cmake
npm install hexo-deployer-git --save  #deploy to git  

hexo clean      
hexo generate    # or hexo g
hexo deploy      # or hexo d
```

打开 `https://lbs0912.github.io/` 可以访问博客界面。


## 域名和DNS
### 域名
申请域名用于博客访问。此处申请域名 `liubaoshuai.com`。

> 已申请的域名包括
> * `liubaoshuai.com`
> * `liubaoshuai.tech`

### 设置CNAME
在Hexo的安装目录下的 `source` 目录下，创建 `CNAME` 文件，并存入申请的域名 `liubaoshuai.com`。

### DNS
推荐使用 [DNSPod](https://www.dnspod.cn/) 进行DNS解析。

此处，由于域名 `http://liubaoshuai.com/` 在阿里云购买，故使用阿里云的 DNS 云解析。DNS 界面进行如下设置。

| 记录类型 |   主机记录 | 记录值  |
|----------|------------|---------|
| CNAME | www | lbs0912.github.io |
| A     |  @  |  192.30.252.154   |
| A     | @   | 192.30.252.153    |

其中A记录为GitHub Page提供的IP地址，可以访问 [GitHub Page](https://help.github.com/articles/github-s-ip-addresses/) 查询最新 `IP` 地址。

最后，执行如下命令，并访问 `liubaoshuai.com` 查看修改效果。

``` bash
hexo clean  
hexo g    # or hexo generate
hexo d    # or hexo deploy
```


* 至此，便可通过访问 `liubaoshuai.com` 来访问自己的博客。
* 访问 `lbs0912.github.io`，会被重定向到 `liubaoshuai.com` 网址。




## Blog 配置

* **[Next Theme 官方配置手册](https://theme-next.iissnan.com/)**

###  Plugin

安装Hexo插件来增强Hexo效果和美化页面。

```bash
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-deployer-git --save  #deploy to git
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked --save
npm install hexo-renderer-stylus --save
npm install hexo-generator-feed --save   # RSS
npm install hexo-generator-sitemap --save #sitemap
```

### 添加Meta信息
`Hexo` 默认的文件头只有`title`、`date`、`tags` 属性，生成的 `html` 缺少 `Meta`信息，不利于搜索引擎收录。建议自行在文件头中添加 `keywords` 和 `description` 属性。`categories` 属性可自行选择是否添加。

```
---
title: ##文章标题
date: ##时间，格式为 YYYY-MM-DD HH:mm:ss
categories: ##分类
tags: ##标签，多标签格式为 [tag1,tag2,...]
keywords: ##文章关键词，多关键词格式为 keyword1,keywords2,...
description: ##文章描述
---
```

文件头模板如上所示，一个文件头实例如下所示。

```
---
title: 这是一篇测试文章
date: 2015-03-21 15:13:48
categories: Hexo
tags: [Hexo,测试]
keywords: Hexo,文章,测试
description: 这是一篇测试文章，用于测试Hexo文章文件头。
---
```

需要注意的是，多个标签也可采用如下写法

```
tags:
  - Testing Tag
  - Another Tag
```

### 分页功能

在 Hexo 安装目录下打开 `_config.yml`，添加如下配置， 为博客添加分页功能。

```
# Plugins
index_generator:
  path: ''
  per_page: 8 ##首页默认8篇文章标题，如果值为0不分页
  order_by: -date
archive_generator:
  per_page: 8 ##归档页面默认8篇文章标题，如果值为0不分页
  yearly: true ##生成年视图
  monthly: true ##生成月视图
tag_generator:
  per_page: 8 ##标签页面默认8篇文章，如果值为0不分页
category_generator: 
  per_page: 8 ##分类页面默认8篇文章，如果值为0不分页
```

### Hexo 主题配置
访问如下链接，查看 Hexo 主题列表
* [Hexo Themes List](https://hexo.io/themes/)
* [Hexo Themes List on GitHub](https://github.com/hexojs/hexo/wiki/Themes) 

#### Next Theme

* 参考 [Next Theme | github](https://github.com/theme-next/hexo-theme-next) 完成基本配置。
* [Next Theme Configure](https://github.com/iissnan/hexo-theme-next)
* 参考 [Next主题美化进阶 | Segmentfault](https://segmentfault.com/a/1190000009544924) 进行定制。
* Install

```
$ cd hexo
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

* 将下载好的 `Huno` 主题放置在 `blog/themes` 目录下。修改 `Hexo` 配置文件`_config.xml`

```
theme: next
```

* Update

```
$ cd themes/next
$ git pull
```

* 设置界面个人头像和网页收藏夹图标

```
# Site favicon
#favicon: /favicon.png
favicon: https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/Blog-20190315/blog-logo-1.jpg

# Site logo
#logo: /avatar.png
logo: https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/Blog-20190315/blog-logo-1.jpg
```

* 作品设计参考网站
 - [Dribbble](https://dribbble.com/)
 - [Behance](https://www.behance.net/)


#### Huno Theme

参考 [Huno Theme | github](https://github.com/letiantian/huno) 完成基本配置。



### 集成第三方服务
* [Next Theme 官方配置手册](https://theme-next.iissnan.com/getting-started.html#third-party-services)


#### 百度统计
* 用户名：15821929853
* 密码：Ab758123aB
* 百度统计-脚本 ID：17082ee15df20dad9762c5512f336eb2

#### 阅读次数统计 LeanCloud
* [leancloud](https://leancloud.cn/)
* [LeanCloud 配置](https://notes.doublemine.me/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)
* 使用 `Github` 第三方账登录 `leancloud`
* APP ID：gksxcfwJlMV3zkhz1pQc7pl2-gzGzoHsz
* APP Key：kjOanp812G7TIGMSQpPCVIhj



#### 搜索功能

* [搜索服务配置](https://theme-next.iissnan.com/third-party-services.html#algolia-search)

`NexT` 主题支持集成 `Swiftype`，`微搜索`，`Local Search` 和 `Algolia` 搜索功能。`Swiftype` 和 `Algolia` 均收费，可以采用 `Hexo` 提供的 `Local Search` 搜索服务，其原理是通过 `hexo-generator-searchdb` 插件在本地生成一个 `search.xml` 文件，搜索的时候从这个文件中根据关键字检索出相应的链接。



#### 博文分享功能
* [hexo next主题为博客添加分享功能](https://blog.csdn.net/lanuage/article/details/78991798)

* 百度分享
```
baidushare:
  type: button  # 需要设置 type: button 
  baidushare: true
```

* likely 分享

```
likely:
  enable: true
  look: light  # available values: normal, light, small, big
  networks:
    twitter: Tweet
    facebook: Share
    linkedin: Link
    #gplus: Plus
    #vkontakte: Share
    #odnoklassniki: Class
    #telegram: Send
    whatsapp: Send
    #pinterest: Pin
```


#### Disqus 评论
* 使用谷歌账户登录Disqus
* shortName：liubaoshuaiBlog

* 之后，在撰写文章时，顶部信息添加 `comments` 字段可控制是否展示评论

```
---
title: Demo
date: 2017-03-10 14:35:26
categories: Demo
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
password: 123456
top: 5
comments: true
---
```


### 404页面
* 推荐使用 [腾讯公益404](http://www.qq.com/404/)，当然也可以自定义 404 页面，例如本博客采用的 404 页面。
* 在 `blog/source` 目录下创建 `404.html`，引入腾讯公益404脚本。(该效果需要部署到服务器才能预览，本地服务无法预览)





### Fork me on GitHub
在 [Fork me on GitHub Theme](https://github.com/blog/273-github-ribbons) 上获取源代码（有多种样式可选），并将 `<a>` 标签的 `href` 属性的链接修改为自己的 [GitHub-lbs0912](https://github.com/lbs0912) 地址。

以 Huno 主题为例，将上述代码添加到 `./themes/huno/layout/_layout.ejs` 文件的`<body>` 标签内即可。

> 修改源代码中`img`标签的样式为`position:fixed`，可以将`Fork me on GitHub`固定于浏览器界面顶部。


### 背景音乐播放设置

参考 [Hexo中播放网易云音乐的实践](http://weqeo.com/2016/10/11/Hexo%E4%B8%AD%E6%92%AD%E6%94%BE%E7%BD%91%E6%98%93%E4%BA%91%E9%9F%B3%E4%B9%90%E7%9A%84%E5%AE%9E%E8%B7%B5/) 完成该部分设置。

以 Huno 主题为例，将网易云音乐播放外链放置在 `./themes/next/layout/_macro/sidebar.swig` 文件中。


```leaf
<% if (!is_home()) { %> 
    <iframe frameborder="no" border="0" marginwidth="0" style="margin-top: 40px;" marginheight="0" width=330 height=86  src="//music.163.com/outchain/player?type=2&id=394653&auto=0&height=66"></iframe>
<% } %>
```


### 为 Blog 添加 README


本博客中，使用了 `Github` 服务器作为托管，博客内容被存储到 `Github` 中。

一般情况下，需要给 `Github` 中每一个项目添加 `README.md` 文件进行说明。

但是，在 `Blog` 项目中，在 `blog\source` 目录下创建的 `README.md` 文件，会被 `hexo` 解析掉，并不会被部署到 `Github` 服务器上。

#### 方式1

* 在博客 `Source` 目录下创建 `README.md` 文件
* 修改博客配置文件的 `skip_render` 字段如下 

```
skip_render: README.md
```

#### 方式2
正确的解决方法如下。
* 把 `README.md` 文件的后缀名改成 `.MDOWN`
* 仍将该文件置于 `blog/source` 文件夹
* 这样可以保证 `hexo` 不会解析该文件，同时 `Github` 也会将其作为`.MD` 文件解析



### 背景效果优化
此处介绍博客背景动态效果图的添加，以及鼠标点击界面出现心形图案的相关设置。

* 下载 [love.js](https://github.com/lbs0912/lbs0912.github.io/blob/master/js/src/love.js) 和 [particle.js](https://github.com/lbs0912/lbs0912.github.io/blob/master/js/src/particle.js) 文件，将其存放至`\themes\huno\source\js\src`目录下。
* 在 `\themes\huno\layout\layout.ejs` 文件末尾，引入上述 2 个 js 文件。

```
<!-- 背景动画 -->
<script type="text/javascript" src="/js/src/particle.js"></script>
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```


### 博文置顶
* 修改 `./node_modules/hexo-generator-index/lib/generator.js` 文件的

```  
var posts = locals.posts.sort(config.index_generator.order_by);
```

为

```
var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
```

* 设置文章置顶：在文件的头部添加 `top` 值，`top` 值越大，文章越靠前。若两篇文章的 `top` 值一样，则按照默认的日期排序。

```
---
title: Webpack Notes - 1
date: 2017-01-19 11:15:48
categories: Front-end
tags: [Webpack,Front-end]
keywords: webpack,front-end 
top: 5
---
```

### 博文收起添加

在 `.MD` 文件中添加如下标识。

```
<!--more-->
```
* 该标识前的会在博客首页展示（可以在该标识前添加简要说明）
* 该标识后的博文会被收起折叠。


### 文章加密阅读
* Ref - [next主题 - 文章加密阅读](https://segmentfault.com/a/1190000009544924#articleHeader23)

* 打开 `themes->next->layout->_partials->head.swig` 文件，添加如下代码

```
<script>
    (function () {
        if ('{{ page.password }}') {
            if (prompt('请输入文章密码') !== '{{ page.password }}') {
                alert('密码错误！');
                if (history.length === 1) {
                    location.replace("http://xxxxxxx.xxx"); // 这里替换成你的首页
                } else {
                    history.back();
                }
            }
        }
    })();
</script>
```

* 之后，在撰写文章时，顶部信息添加 `password` 字段即可

```
---
title: Demo
date: 2017-03-10 14:35:26
categories: Demo
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
password: 123456
top: 5
comments: true
---
```


### 定制CSS
* 在 `.\themes\next\source\css\_custom\custom.styl` 文件中，添加自定义CSS样式。

定制CSS样式后，可以使用如下效果。

```
<span id="inline-blue">Demo</span>， 
<span id="inline-purple">Demo</span>
<span id="inline-green">Demo</span>
<span id="inline-yellow">Demo</span>

<p id="div-border-left-red">Demo</p>
<p id="div-border-left-yellow">Demo</p>
<p id="div-border-left-green">Demo</p>
<p id="div-border-left-blue">Demo</p>
<p id="div-border-left-purple">Demo</p>

<p id="div-border-right-red">Demo</p>
<p id="div-border-right-yellow">Demo</p>
<p id="div-border-right-green">Demo</p>
<p id="div-border-right-blue">Demo</p>
<p id="div-border-right-purple">Demo</p>

<p id="div-border-top-red">Demo</p>
<p id="div-border-top-yellow">Demo</p>
<p id="div-border-top-green">Demo</p>
<p id="div-border-top-blue">Demo</p>
<p id="div-border-top-purple">Demo</p>

<span id="yu-1">动画</span>

<a id="download" href="https://git-scm.com/download/win"><i class="fa fa-download"></i><span> Download Now</span>
</a>

<blockquote class="blockquote-center">引用居中效果</blockquote>
```


#### 链接文本样式修改

将链接文本设置为蓝色，鼠标划过时文字颜色加深，并显示下划线。打开`themes\next\source\css\_custom\custom.styl` 文件 ，添加如下 `css` 样式

```
// Custom styles.
.post-body p a {
  color: #0593d3;
  border-bottom: none;
  &:hover {
    color: #0477ab;
    text-decoration: underline;
  }
}
```

#### 文字增加背景色块

* 参考 [Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background) 完成该部分的设置。

*  使用 `inline-blue`, `inline-purple`, `inline-yellow`,`inline-green` 可以对文字背景色块进行修改。

```
<span id="inline-blue">站点配置文件</span>， 
<span id="inline-purple">主题配置文件</span>
```

#### 图形边框效果
参考 [Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background) 完成该部分的设置。

#### 引用边框变色
参考[Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background)

#### 引用居中效果

```
<blockquote class="blockquote-center">引用居中效果</blockquote>
```


#### Font Awesome 使用
* [Font Awesome](http://fontawesome.dashgame.com/)

使用 `Font Awesome` 图标时，只需要使用 CSS 前缀 `fa`，再加上图标名称即可。

```
<i class="fa fa-pencil"></i> fa-pencil
<i class="fa fa-pencil-square-o"></i> fa-pencil-square-o
<i class="fa fa-camera-retro"></i> fa-camera-retro
<i class="fa fa-share-square-o"></i> fa-share-square-o
<i class="fa fa-tag"></i> fa-tag
<i class="fa fa-video-camera"></i> fa-video-camera
<i class="fa fa-ban"></i> fa-ban
<i class="fa fa-code"></i> fa-code
<i class="fa fa-cloud"></i> fa-cloud
<i class="fa fa-pie-chart"></i> fa-pie-chart
<i class="fa fa-thumbs-o-up"></i> fa-thumbs-o-up
<i class="fa fa-chain"></i> fa-chain
<i class="fa fa-link"></i> fa-link
<i class="fa fa-edit"></i> fa-edit
<i class="fa fa-share-alt"></i> fa-share-alt
<i class="fa fa-jsfiddle"></i> fa-jsfiddle
<i class="fa fa-git"></i> fa-git
<i class="fa fa-codepen"></i> fa-codepen

```


### 主页图片展示

* 新建博文，设置 `type: "picture"`，使用 `{\% gp x-x \%} ... {\% endgp \%}` 标签引用要展示的图片地址。

```
---
title: HomePageImage
type: "picture"
top: 999999999999999
date: 2018-09-12 16:50:21
categories: HomePageImage
tags: HomePageImage
---

{% gp 5-3 %}
![my-riding-bike](http://ojxk3q6gs.bkt.clouddn.com/my-riding-bike.jpg)
![football](http://ojxk3q6gs.bkt.clouddn.com/football.jpg)
![sjtu-title-3](http://ol3kbaay9.bkt.clouddn.com/sjtu-title-3.jpg)
![front-end-logo-1](http://ol3kbaay9.bkt.clouddn.com/front-end-logo-1.jpg)
![java-c-vs123](http://ol3kbaay9.bkt.clouddn.com/java-c-vs123.jpg)
{% endgp %}
```

* 图片展示效果

`{\% gp 5-3 \%}` 用于设置图片展示效果，参考 `theme/next/scripts/tags/group-pictures.js ` 注释示意图。

* 修复图片展示

博客主页目前可以正常显示上步骤中设置的图片模式效果，但是点击进入后，图片显示效果会丢失，所以需修改
`themes\next\source\css\_common\components\tags\group-pictures.styl` 文件中的以下样式

```
.page-post-detail .post-body .group-picture-column {
  // float: none;
  margin-top: 10px;
  // width: auto !important;
  img { margin: 0 auto; }
}
```

### 博文底部标签样式

* 将博文底部的表情样式，从改为 `#` 改为 `Font Awesome` 图标的标签样式。
* 修改模板 `/themes/next/layout/_macro/post.swig`，搜索 `rel="tag">#`，将其中的 `#` 换成 `<i class="fa fa-tag"></i>`。

### 首页-简历和相册分类

#### 创建

* Create

```
hexo new page resume 
hexo new page album
```

* Configure

```
menu:
  home: / || home
  resume: /resume/ || child
  album: /album || picture-o
```


> 在简历和相册对应的 `index.md` 文件头部添加 `comments: false` 可以关闭评论列表。

#### 简历配置

除了用 `markdown` 书写个人简历外，也可以用 `HTML` 书写个人简历。此时，需要在文件头部添加不进行渲染指令。

```
---
layout: false
title: 个人简历
---
<!doctype html>
<html lang="zh">
<head>
<!-- resume code here-->
```

#### 相册配置

##### 参考资料
* [Next Theme 相册配置](https://timding.top/2017/09/18/Hexo-NexT-%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E7%9B%B8%E5%86%8C-%E4%BA%8C/) 
* [搭建Hexo 相册](https://juejin.im/post/5b8bc953518825284910dcdd)

##### Tip
* 在 `album` 目录下添加 `assets/empty.jpg`，作为图片展示的占位图。
* 本相册配置中是将图片存放在 `github`的，其访问链接是 `https://raw.githubusercontent.com` 开头的，并不是图片的存储地址。因此，`album/ins.js` 中图片链接地址为

```
var minSrc = 'https://raw.githubusercontent.com/lbs0912/HexoBlog/master/source/album/photos_configure/min_photos/' + data.link[i];

var src = 'https://raw.githubusercontent.com/lbs0912/HexoBlog/master/source/album/photos_configure/photos/' + data.link[i];
```

## 最后
* 记录，成为更好的自己。
* 本博客全部配置信息可在 [HexoBlog | lbs0912-github](https://github.com/lbs0912/HexoBlog) 查看。
