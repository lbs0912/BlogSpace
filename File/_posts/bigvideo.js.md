---
title: BigVideo.js实现网页全屏视频背景
date: 2016-09-27 10:32:27
tags: [Front-end, BigVideo]
categories: Front-end
---
## BigVideo.js简介
**BigVideo.js**是一款[jQuery](https://jquery.com/)插件，它可以为网页创建一个自动适配（Fit-to-Fill）的视频背景，对于一些不允许背景视频自动播放的设备，也可以用来显示一张背景图片。
<!-- more -->
关于BigVideo.js更详细的介绍，请参考
- [BigVideo.js Demo](http://dfcb.github.io/BigVideo.js/)
- [BigVideo.js on Github](https://github.com/dfcb/BigVideo.js)

本文中展示的网页效果图如下所示。
![](http://oda53d5ps.bkt.clouddn.com/bigvideo.gif)

本作品的更多信息，请参考
- [Demo](http://oda53d5ps.bkt.clouddn.com/bigvideo.gif)
- [Demo Code on Github](https://github.com/lbs0912/BigVideo.js-Fullpage-Video-Background)

本作品具有如下特点：
- 多种视频格式（mp4,webm,ogg）
- 具有视频播放标签，可以手动选择视频背景
- 可以对内容进行显示和隐藏
- 支持视频的播放与暂停
- 支持多视频播放
- 针对移动设备，默认显示一张背景图片，不会自动播放视频



## BigVideo.js使用
### 使用bower下载BigVideo.js和其依赖的文件
若安装了**bower**，可以使用`bower`来下载BigVideo.js和其依赖的文件。打开CMD并将路径切换到工程目录下，输入如下指令即可。

``` 
 bower install BigVideo.js

```
该指令会下载并安装BigVideo到工程目录下的`bower_components`目录下。
下面只需在HTML文件中使用`script`标签引入这些文件即可，当然，也可以使用`RequireJS`引入。
以`script`标签引入方式为例，先引入所需的js文件。由于BigVideo插件依赖于`vidoe.js`，`jQuery`和`jQuery UI`，因此，也在引入文件时，也需要引入`vidoe.js`,`jquery.min.js`和`jquery-ui.min.js`文件。

```  html
  <!-- BigVideo Dependencies -->
    <script tyep="text/javascript" 
        src="./bower_components/jquery/dist/jquery.min.js">
    </script>
    <script tyep="text/javascript" 
        src="./bower_components/jquery-ui/jquery-ui.min.js">
    </script>
    <script tyep="text/javascript" 
        src="./bower_components/video.js/dist/video.js">
    </script>
    <script tyep="text/javascript" 
        src="./bower_components/BigVideo/lib/bigvideo.js">
    </script>

```
然后，再引入BigVideo所需的`css`样式表

``` html
<link rel="stylesheet" type="text/css"
         href="./bower_components/BigVideo/css/bigvideo.css">
```
### 创建BigVideo对象并进行相关设置
#### 全屏播放视频
``` html
<script>
	$(function() {
	    var BV = new $.BigVideo();
	    BV.init();
	    BV.show("http://oda53d5ps.bkt.clouddn.com/mountain.mp4");
	});
</script>
```


考虑到多浏览器，可以包含多种格式的视频。由于Safari浏览器对`webm`格式的视频支持不太好，因此首先包含mp4格式的视频。

```html
	<script>
	$(function() {
	    var BV = new $.BigVideo({useFlashForFirefox:false});
		BV.init();
	    BV.show([
	        { type: "video/mp4",  src: "http://oda53d5ps.bkt.clouddn.com/mountain.mp4" },
	        { type: "video/webm", src: "http://oda53d5ps.bkt.clouddn.com/mountain.webm" },
	        { type: "video/ogg",  src: "http://oda53d5ps.bkt.clouddn.com/ogg-19M.ogg" }
	    ]);
	});
	</script>
```
#### 背景视频
若需将视频以背景形式无声播放，可以将`BigVideo`的`ambient`设置为`true`。

```html
	<script>
	$(function() {
	    var BV = new $.BigVideo();
	    BV.init();
	    BV.show('http://vjs.zencdn.net/v/oceans.mp4',{ambient:true});
	});
	</script>
```

相应地，将一系列视频以背景形式无声播放的设置如下所示。

```html
	<script>
	$(function() {
	    var BV = new $.BigVideo();
	    BV.init();
	    BV.show([
	        { type: "video/mp4",  src: "http://oda53d5ps.bkt.clouddn.com/mountain.mp4" },
	        { type: "video/webm", src: "http://oda53d5ps.bkt.clouddn.com/mountain.webm" },
	        { type: "video/ogg",  src: "http://oda53d5ps.bkt.clouddn.com/ogg-19M.ogg" }
	    ],{ambient:true});
	});
	</script>
```
将视频作为背景时，为了正常显示HTML中的元素，可以将这些元素设置为相对定位，即

``` CSS
	position:relative;
```



### BigVideo对移动设备的处理
移动设备中不允许视频自动播放，针对此情况，先使用[Modernizr](https://modernizr.com/)来检测是否为触摸屏设备，若为触摸屏移动设备，BigVideo将使用一张大图片作为背景来替代视频背景。`Modernizr`的使用方法如下所示。

```html
<!--对移动设备检测，包含modernizr库-->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/modernizr/2.8.3/modernizr.min.js">
    </script>

```

``` html
<script>
    $(function(){         
        var BV = new $.BigVideo();
        BV.init();
                           
        if (Modernizr.touch) {  //触摸设备判断
            //对于触摸设备，显示一张图片
            BV.show("http://oda53d5ps.bkt.clouddn.com/riding.png");   
        } else {
            BV.show("http://oda53d5ps.bkt.clouddn.com/mountain.mp4",{ambient:true});
        }
    });
</script>
```
效果图如下图所示。
![](http://oda53d5ps.bkt.clouddn.com/Image%201.png)


### 视频播放列表的添加
添加视频播放列表，可以手动切换背景视频。具体实现过程中如下所示。
1.创建一个无序列表，每个`li`标签中放置一个`a`元素，并设置其`href`属性为视频源地址；

``` html
	<div class="navigation">
		<ul class="playlist-video">
			<li class="li-background"><a class="playlist-btn" 
					href="http://oda53d5ps.bkt.clouddn.com/mountain.mp4">Video #1
				</a>
			</li>
			<li class="li-background"><a class="playlist-btn" 
			        href="http://oda53d5ps.bkt.clouddn.com/summer.mp4">Video #2
			    </a>
			</li>
			<li class="li-background"><a class="playlist-btn"  
					href="http://oda53d5ps.bkt.clouddn.com/nanjing.mp4">Video #3
				</a>
			</li>
		</ul>
	</div>

```
2.对`li`元素绑定点击事件，将获取到的`a`标签`href`属性中的链接地址传递给`BigVideo`中创建的`BV`对象。

``` javascript
    //视频源选择        
    $(".playlist-btn").on("click",function(e){
       	e.preventDefault();   //阻止默认的href跳转
       	BV.show($(this).attr("href"),{ambient:true});   //载入视频源
       	//console.log($(this).attr("href"));     	
    });
```
### 视频播放与暂停的设定
由于`BigVideo.js`是建立在[Video.js](http://videojs.com/)的基础上的,因此，可以直接使用`Video.js`的API。例如，使用`getPlayer()`方法来实现视频的播放与暂停。具体实现过程中如下所示。
1.创建一个复选框，并设置为非移动设备下默认选中；

```html
<li class="li-background" style="font-weight: bold;color:white">
	<input class="play-pause" type="checkbox" name="Video Playing" 
		value="Video Playing" checked="true">
	Playing
</li>
```
2.使用`getPlayer()`方法来实现视频的播放与暂停

```javascript
   //视频暂停/播放选择
    $(".play-pause").on("click",function(){
        if($(".play-pause").is(":checked")){   //是否播放视频
            BV.getPlayer().play();
        }
        else{
            BV.getPlayer().pause();
        }
    });  
```
### 内容的显示与隐藏
本部分功能实现较为简单，仅作简要说明。
利用`display:none`来实现内容的隐藏，利用`display:block`来实现内容的显示。在内容隐藏状态下，本案例中设置了定时3秒后选择框的自动显示。




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 


