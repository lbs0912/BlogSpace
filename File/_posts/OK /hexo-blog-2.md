---
title: 使用GitHub和Hexo搭建免费静态博客(二)
date: 2017-01-17 19:21:00
tags: [Front-end,hexo]
categories: Front-end
---

## 前言
本文主要介绍使用GitHub和[Hexo](https://hexo.io/)搭建个人博客的步骤和配置过程。

本文为该系列博文的第2篇，主要介绍Hexo参数的配置和进阶使用技巧，包括Hexo主题的安装与配置，自定义CSS样式，博客分页功能的设置，文章阅读次数统计功能的添加，多说评论插件的安装，个人简历的创建等方面。

<!--more-->

## 更新历史
* 博文发表 - 2016-11-12
* 添加个人简历创建章节 - 2017-01-14
* 添加自定义CSS章节 - 2017-02-02
* 博文排版更新 - 2017-02-06


## Hexo参数配置
### 安装Hexo插件
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
### Hexo常用命令
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

### 为博文添加Meta信息
Hexo默认的文件头只有`title`、`date`、`tags`属性，生成的html会缺少`Meta`信息，不利于搜索引擎收录。建议自行在文件头中添加`keywords`和`description`属性。`categories`属性可自行选择是否添加。
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

### 添加分页功能
```
# Plugins
index_generator:
  per_page: 8 ##首页默认8篇文章标题，如果值为0不分页
archive_generator:
  per_page: 8 ##归档页面默认8篇文章标题，如果值为0不分页
  yearly: true ##生成年视图
  monthly: true ##生成月视图
tag_generator:
  per_page: 8 ##标签页面默认8篇文章，如果值为0不分页
category_generator: 
  per_page: 8 ##分类页面默认8篇文章，如果值为0不分页
```

### Hexo主题配置
选择Hexo主题，进行博客页面美化。访问 [Hexo Themes List](https://hexo.io/themes/) 和 [Hexo Themes List on GitHub](https://github.com/hexojs/hexo/wiki/Themes) 查看所有主题。
####  Yilia主题配置
此处以[Yilia](https://github.com/litten/hexo-theme-yilia)主题为例进行说明。

* Install yilia

```cmake
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia  
```

* Configure Hexo

在Hexo安装目录下（此处以`E:\MyBlogRepository`目录为例）打开`_config.yml`文件，进行如下修改。

```cmake
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: yilia
```

* Update Yilia

```cmake
cd themes/yilia
git pull
```

* 参考[Yilia](https://github.com/litten/hexo-theme-yilia)进行更多配置，添加标签等功能。

####  Huno主题配置
* 参考 [Huno GitHub Page](https://github.com/letiantian/huno) 完成主题安装与配置。
* 设置界面个人头像和网页收藏夹图标
```
# Site favicon
#favicon: /favicon.png
favicon: http://ojx8u3g1z.bkt.clouddn.com/mylogo-1.png

# Site logo
#logo: /avatar.png
logo: http://ojx8u3g1z.bkt.clouddn.com/mylogo-1.png
```

* 作品设计参考网站
 - [Dribbble](https://dribbble.com/)
 - [Behance](https://www.behance.net/)

####  Next主题配置
参考[Next GitHub Page](https://github.com/iissnan/hexo-theme-next)和[Next Document](http://theme-next.iissnan.com/)完成主题安装和相关配置。

## Hexo进阶使用
### 评论工具安装
目前常用的评论系统有[Disqus](https://disqus.com/)和[多说](http://duoshuo.com/)。

* [Disqus](https://disqus.com/)
>[Disqus](https://disqus.com/) offers the best add-on tools for site owners to power discussions, increase engagement, and earn revenue. --- [Disqus](https://disqus.com/) 

* [多说](http://duoshuo.com/) 
>[多说](http://duoshuo.com/)——下载量第1的评论系统，让评论更活跃，让网站与微博紧紧相连，提升流量，抵御垃圾评论，并永久免费。 --- [多说](http://duoshuo.com/)

此处，以多说评论系统为例，进行说明。

* 注册[多说](http://duoshuo.com/)账号并完成相关设置，获取多说域名`http://liubaoshuai.duoshuo.com`
* 在Hexo根目录下的配置文件`_config.yml`中输入`duoshuo_shortname`

```
# Duoshuo Comment Support
duoshuo_shortname: liubaoshuai
```

* 开启多说热评文章

```
# Duoshuo Comment Support
duoshuo_shortname: liubaoshuai
duoshuo_hotartical: true
```

* 设置多说评论样式

在多说网站的后台管理界面中，`自定义CSS`对话框中进行如下样式表设置

```
/*-------------访客底部----------------*/
.ds-recent-visitors {
    margin-bottom: 200px;
}
@media (max-width: 768px) {
    .ds-recent-visitors {
        margin-bottom: 440px;
    }
}
/*-------------非圆角----------------*/
#ds-reset .ds-rounded {
    border-radius: 0px;
}
.theme-next #ds-thread #ds-reset .ds-textarea-wrapper {
    border-top-right-radius: 0px;
    border-top-left-radius: 0px;
}
.theme-next #ds-thread #ds-reset .ds-post-button {
    border-radius: 0px;
}
.ds-post-self xmp {
    word-wrap: break-word;
}
/*-------------访客----------------*/
#ds-reset .ds-avatar img,
#ds-recent-visitors .ds-avatar img {
    width: 54px;
    height: 54px; /*设置图像的长和宽，这里要根据自己的评论框情况更改*/
    border-radius: 27px; /*设置图像圆角效果,在这里我直接设置了超过width/2的像素，即为圆形了*/
    -webkit-border-radius: 27px; /*圆角效果：兼容webkit浏览器*/
    -moz-border-radius: 27px;
    box-shadow: inset 0 -1px 0 #3333sf; /*设置图像阴影效果*/
    -webkit-box-shadow: inset 0 -1px 0 #3333sf;
    -webkit-transition: 0.4s;
    -webkit-transition: -webkit-transform 0.4s ease-out;
    transition: transform 0.4s ease-out; /*变化时间设置为0.4秒(变化动作即为下面的图像旋转360读）*/
    -moz-transition: -moz-transform 0.4s ease-out;
}
/*-------------访客悬浮在头像----------------*/
#ds-reset .ds-avatar img:hover,
#ds-recent-visitors .ds-avatar img:hover {
    box-shadow: 0 0 10px #fff;
rgba(255, 255, 255, .6), inset 0 0 20 px rgba(255, 255, 255, 1);
    -webkit-box-shadow: 0 0 10px #fff;
rgba(255, 255, 255, .6), inset 0 0 20 px rgba(255, 255, 255, 1);
    transform: rotateZ(360deg); /*图像旋转360度*/
    -webkit-transform: rotateZ(360deg);
    -moz-transform: rotateZ(360deg);
}
#ds-thread #ds-reset .ds-textarea-wrapper textarea {
    background: url(http://ww4.sinaimg.cn/small/649a4735gw1et7gnhy5fej20zk0m8q3q.jpg) right no-repeat;
}
#ds-recent-visitors .ds-avatar {
    float: left
}
/*-------------隐藏版权----------------*/
#ds-thread #ds-reset .ds-powered-by {
    display: none;
}
```

### 网站统计功能添加
此处使用 `LeanCloud` 统计文章阅读数，使用`不蒜子`统计站点的浏览量`PV`和访客数`UV`。

#### 文章阅读次数统计（LeanCloud)

* 参考[为NexT主题添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud) 和 [Hexo的NexT主题个性化：添加文章阅读量](http://www.jeyzhang.com/hexo-next-add-post-views.html)完成该部分设置，获取
	- App ID: l8jW2G9Iet6crSHh9Q0SVowK-gzGzoHsz
	- App Key: nPgO4ivv48evqpJiyp1SxxWS

* 在Huno主题的配置文件中进行如下设置
```
# LeanCloud Statistics Support
leancloud_visitors:
  enable: true
  app_id: l8jW2G9Iet6crSHh9Q0SVowK-gzGzoHsz
  app_key: nPgO4ivv48evqpJiyp1SxxWS
```
#### 不蒜子统计站点访问
参考[不蒜子官网](http://busuanzi.ibruce.info/)和[不蒜子网站计数](http://ibruce.info/2015/04/04/busuanzi/)完成配置。此处将不蒜子的统计信息显示于文章标题段，在Huno主题下的`layout\_partials`目录中的`article.ejs`文件中添加如下设置

```
<!-- 不蒜子站点访问统计 -->
<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js">
</script>

<span id="busuanzi_container_site_pv">
	View <span id="busuanzi_value_site_pv"></span> times
</span>
```
#### 百度统计
使用[百度统计](http://tongji.baidu.com/web/welcome/login)或者[谷歌统计](https://www.google.com/intl/zh-CN/analytics/)等工具，也可以统计网站的访问量。

### 404页面
推荐使用[腾讯公益404](http://www.qq.com/404/)，当然也可以自定义404页面，例如本博客采用的404页面。

###  mathjax数学公式

###  图床
推荐使用七牛（10G空间，免费），还可以使用 Yupoo（100m免费空间）

### 添加Fork me on GitHub
在[Fork me on GitHub Theme](https://github.com/blog/273-github-ribbons)上获取源代码（有多种样式可选），并将`<a>`标签的`href`属性的链接修改为自己的[GitHub](https://github.com/lbs0912)地址。

以Next主题为例，将上述代码添加到`./themes/next/layout/_layout.swing`文件的末尾即可。

> 修改源代码中`img`标签的样式为`position:fixed`，可以将`Fork me on GitHub`固定于浏览器界面顶部。

### 博客主页图片模式展示
此处介绍如何在博客主页进行图片模式展示。

* 新建博文，设置`type: "picture"`，使用`{\% gp x-x \%} ... {\% endgp \%}`标签引用要展示的图片地址
```
---
title: Naruto-Pictures
categories: [图片]
tags: [picture,naruto]
date: 2016-09-02 14:36:04
keywords: picture,naruto
type: "picture"
top: 999
---
{% gp 5-3 %}
![my-riding-bike](http://ojxk3q6gs.bkt.clouddn.com/my-riding-bike.jpg)
![football](http://ojxk3q6gs.bkt.clouddn.com/football.jpg)
![programing](http://ojxk3q6gs.bkt.clouddn.com/programing.jpg)
![arm-linux](http://ojxk3q6gs.bkt.clouddn.com/arm-linux.jpg)
![sjtu-title](http://ojxk3q6gs.bkt.clouddn.com/sjtu-title.jpg)
{% endgp %}
```

* 图片展示效果
`{\% gp 5-3 \%}`用于设置图片展示效果，参考` theme/next/scripts/tags/group-pictures.js `注释示意图。

* 修复图片展示
博客主页目前可以正常显示上步骤中设置的图片模式效果，但是点击进入后，图片显示效果会丢失，所以需修` themes\next\source\css\_common\components\tags\group-pictures.styl`文件中的以下样式
```
.page-post-detail .post-body .group-picture-column {
  // float: none;
  margin-top: 10px;
  // width: auto !important;
  img { margin: 0 auto; }
}
```
* 至此，博客主页图片模式显示效果已经完成，可以访问[博客主页](http://liubaoshuai.com/)查看效果。
* 参考资料：[使用Hexo基于GitHub-Pages搭建个人博客（三）](http://ehlxr.me/2016/08/30/%E4%BD%BF%E7%94%A8Hexo%E5%9F%BA%E4%BA%8EGitHub-Pages%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%B8%89%EF%BC%89/)



### 博文压缩
目前，有两个插件可以压缩博文：`hexo-all-minifier` 插件和 `gulp` 插件。`hexo-all-minifier` 插件虽然使用比较简单，而且可以压缩图片，但是发现对文章缩进（输入法全拼模式下按 `Tab`）不支持，所以暂时使用第二种压缩手段。

#### hexo-all-minifier配置使用
```
npm install hexo-all-minifier --save
```
安装`hexo-all-minifier`后，运行`hexo g` 命令生成博文时候，就会自动压缩HTML，JS，CSS，图片等文件，详情参见[插件介绍](https://github.com/chenzhutian/hexo-all-minifier)。

#### gulp插件配置使用

```
npm install gulp -g
npm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean gulp --save
```

在 博客根目录下（即`package.json` 同级目录下），新建 `gulpfile.js` 并输入以下内容

```
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
// 压缩 public 目录 css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss())
        .pipe(gulp.dest('./public'));
});
// 压缩 public 目录 html
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
    .pipe(htmlclean())
    .pipe(htmlmin({
         removeComments: true,
         minifyJS: true,
         minifyCSS: true,
         minifyURLs: true,
    }))
    .pipe(gulp.dest('./public'))
});
// 压缩 public/js 目录 js
gulp.task('minify-js', function() {
    return gulp.src('./public/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 执行 gulp 命令时执行的任务
gulp.task('default', [
    'minify-html','minify-css','minify-js'
]);
```

执行 `hexo g && gulp`生成博文并根据 `gulpfile.js` 中的配置，对 `public` 目录中的静态资源文件进行压缩。


### 背景音乐播放设置
参考[Hexo中播放网易云音乐的实践](http://weqeo.com/2016/10/11/Hexo%E4%B8%AD%E6%92%AD%E6%94%BE%E7%BD%91%E6%98%93%E4%BA%91%E9%9F%B3%E4%B9%90%E7%9A%84%E5%AE%9E%E8%B7%B5/)完成该部分设置。

### 修改文章内链接文本样式
将链接文本设置为蓝色，鼠标划过时文字颜色加深，并显示下划线。打开`themes\next\source\css\_custom\custom.styl`文件 ，添加如下`css` 样式
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

### 给 Github 添加 README
默认情况下，Github中每一个项目，我们希望都有一份`README.md`的文件来作为项目的说明，但是我们在项目根目录下的`blog\source`目录下创建一份`README.md`文件，写好说明介绍，部署的时候，这个`README.md`会被`hexo`解析掉，而不会被解析到`Github`中去的。

正确的解决方法如下。把`README.md`文件的后缀名改成`.MDOWN`然后扔放置于`blog/source`文件夹下即可，这样hexo不会解析，Github也会将其作为MD文件解析。

### 菜单栏中标签与侧边栏中标签关联的问题
参考[Hexo-NexT搭建个人博客（三）](https://neveryu.github.io/2016/11/11/hexo-next-three/)完成该部分的设置。

### 文字增加背景色块

* 参考[Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background)完成该部分的设置。

*  使用`inline-blue`, `inline-purple`, `inline-yellow`,`inline-green`可以对文字背景色块进行修改。
```
<span id="inline-blue">站点配置文件</span>， 
<span id="inline-purple">主题配置文件</span>
```

### 图形边框效果
参考[Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background)完成该部分的设置。

### 引用边框变色
参考[Hexo博客设置进阶](http://blog.junyu.pro/posts/0010-hexo-learn-from-Never-yu.html#background)完成该部分的设置。



### 背景效果优化
此处介绍博客背景动态效果图的添加和鼠标点击界面出现心形图案的相关设置。
* 下载[love.js](https://github.com/lbs0912/lbs0912.github.io/blob/master/js/src/love.js)和[particle.js](https://github.com/lbs0912/lbs0912.github.io/blob/master/js/src/particle.js)文件，将其存放至`\themes\next\source\js\src`目录下。
* 在`\themes\next\layout\_layout.swig`文件末尾，引入上述2个js文件。
```
 <!-- 背景动画 -->
  <script type="text/javascript" src="/js/src/particle.js"></script>
  <!-- 页面点击小红心 -->
  <script type="text/javascript" src="/js/src/love.js"></script>
```
* 更新博客配置，查看效果即可。

### 博文置顶
* 修改`./node_modules/hexo-generator-index/lib/generator.js`文件的`  var posts = locals.posts.sort(config.index_generator.order_by);`语句为
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
* 设置文章置顶：在文件的头部添加`top`值，top值越大，文章越靠前。若两篇文章的top值一样，则按照默认的日期排序。
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


### 博客站内搜索服务
参考[Hexo博客添加站内搜索](http://www.ezlippi.com/blog/2017/02/hexo-search.html)完成该部分设置。
#### Local Search
NexT主题支持集成 `Swiftype`、 `微搜索`、`Local Search` 和 `Algolia`搜索功能。Swiftype和Algolia都只有一段时间的试用期，可以采用Hexo提供的Local Search搜索服务，其原理是通过`hexo-generator-search`插件在本地生成一个`search.xml`文件，搜索的时候从这个文件中根据关键字检索出相应的链接。

* 安装插件
```cmake
npm install hexo-generator-search --save
npm install hexo-generator-searchdb --save
```

* 启用搜索
在Next主题下的站点配置文件中添加如下信息。
```cmake
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

#### Swiftype 搜索
参考[第三方服务集成——Swiftype](http://theme-next.iissnan.com/third-party-services.html#swfitype)完成该部分设置。Swiftype 搜索目前已收费，推荐使用微搜索服务。

#### Algolia搜索
参考[Hexo+Next主题集成Algolia搜索](http://blog.niices.com/Hexo-Next-Algolia-Search/)完成该部分设置。Algolia搜索目前已收费，推荐使用微搜索服务。

#### 微搜索
该服务可以免费使用


###  RSS订阅
```cmake
npm install hexo-generator-feed --save   # RSS
```
执行上述命令并修改Hexo的安装配置文件`_config.yml`如下。

``` cmake
#Extensions
##Plugins: http://hexo.io/plugins/
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
type: atom
path: atom.xml
limit: 20  

```
### Sitemap站点地图创建
```cmake
npm install hexo-generator-seo-friendly-sitemap --save
```
执行上述命令行，并在Hexo根目录下的配置文件中配置`sitemap`参数。

### 首页增加简历和相册链接
运行下面代码，会在source目录下生成`resume`文件夹，其中会自动生成`index.md`文件。
```
hexo new page resume    
```
在博客主题配置文件中设置menu和menu_icons参数，设定相应的图标。
```
menu:
  home: /
  resume: /resume
  album: /album

menu_icons:
  enable: true
  resume: user
  album: picture-o
```
在Next主题下的`languages`文件夹中，打开语言配置文件（此处以`zh-Hans.yml`为例），修改对应的设置。
```
menu:
  resume: 简历
  album: 相册
```

> 在`index.md`文件头部添加`comments: false`可以关闭评论列表。


此外，除了用`markdown`书写个人简历外，也可以用HTML书写个人简历。此时，需要在文件头部添加不进行渲染指令。
```
---
layout: false
title: 个人简历
---
<!doctype html>
<html lang="zh">
<head>
...
```

至此，简历和相册的首页图表和链接已经创建完成，关于简历的创建和相册图片的设置，参见本站点博文[使用GitHub和Hexo搭建免费静态博客(三)](http://liubaoshuai.com/2017/01/17/hexo-blog-3/)。


## 后续介绍
关于简历的创建和相册中图片的设置，参考该系列博文后续介绍。

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