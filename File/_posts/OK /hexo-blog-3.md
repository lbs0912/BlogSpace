---
title: 使用GitHub和Hexo搭建免费静态博客(三)
date: 2017-01-17 23:21:00
tags: [Front-end,hexo]
categories: Front-end
---

## 前言
本文主要介绍使用GitHub和[Hexo](https://hexo.io/)搭建个人博客的步骤和配置过程。

本文为该系列博文的第3篇，主要介绍个人简历的创建和相册图片格式的设置，对Hexo使用过程中遇到的问题进行记录，对该系列博文的前2篇进行补充和完善。

<!--more-->

## 更新历史
* 博文发表 - 2016-11-12
* 添加Hexo使用过程中遇到的问题 - 2017-01-14
* 博文排版更新 - 2017-02-06

## 个人简历的创建

## 相册图片格式的设置

### 相册选项的创建
该部分设置参见本系列博客之[使用GitHub和Hexo搭建免费静态博客(二)-首页增加简历和相册链接](http://liubaoshuai.com/2017/01/17/hexo-blog-2.html)

### 相册图片格式设置
#### Node.js实现图片一键上传至七牛云

* 在博客根目录下创建`photos`目录，用于存放待上传的图片
* 在`source/album`目录下创建`tools.js`文件，输入如下内容。`qiniu.conf.ACCESS_KEY`和`qiniu.conf.SECRET_KEY`需要替换成自己的七牛云账号的参数。
* 运行`node tools.js`，即可将图片一键上传至七牛云


```
 const fs = require("fs");
    const path = "../../photos";
    var qiniu = require("qiniu");

    //需要填写你的 Access Key 和 Secret Key
    qiniu.conf.ACCESS_KEY = 'iOmghigQJeF1gtXrH_G7IQtRZ66TY4LeaCOxrALz';
    qiniu.conf.SECRET_KEY = 'MM2jxEhh7jlhrhTE14lhgPkruxepfeTVRANsdsft';

    //要上传的空间
    bucket = 'myblogalbum';

    //构建上传策略函数
    function uptoken(bucket, key) {
      var putPolicy = new qiniu.rs.PutPolicy(bucket+":"+key);
      return putPolicy.token();
    }

    //构造上传函数
    function uploadFile(uptoken, key, localFile) {
        var extra = new qiniu.io.PutExtra();
        qiniu.io.putFile(uptoken, key, localFile, extra, function(err, ret) {
          if(!err) {
            // 上传成功， 处理返回值
            console.log('upload success : ',ret.hash, ret.key);
          } else {
            // 上传失败， 处理返回代码
            console.log(err);
          }
      });
    }

    /**
     * 读取文件后缀名称，并转化成小写
     * @param file_name
     * @returns
     */
    function getFilenameSuffix(file_name) {
      if(file_name=='.DS_Store'){
        return '.DS_Store';
      }
        if (file_name == null || file_name.length == 0)
            return null;
        var result = /\.[^\.]+/.exec(file_name);
        return result == null ? null : (result + "").toLowerCase();
    }


    fs.readdir(path, function (err, files) {
        if (err) {
            return;
        }
        var arr = [];
        (function iterator(index) {
            if (index == files.length) {
                fs.writeFile("./output.json", JSON.stringify(arr, null, "\t"));
                return;
            }

            fs.stat(path + "/" + files[index], function (err, stats) {
                if (err) {
                    return;
                }
                if (stats.isFile()) {
                  var suffix = getFilenameSuffix(files[index]);
                  if(!(suffix=='.js'|| suffix == '.DS_Store')){
                    //要上传文件的本地路径
                    filePath = path+'/'+files[index];
                    console.log('抓取到文件: '+files[index]);
                    //上传到七牛后保存的文件名
                    key = files[index];
                    //生成上传 Token
                    token = uptoken(bucket, key);
                    // 异步执行
                    uploadFile(token, key, filePath);
                    arr.push(files[index]);
                }

                          }
                iterator(index + 1);
            })
        }(0));
    });

    console.log("'output.json' file has been created successfully...");

```



#### 相册图片格式设置

参考如下模板对相册图片格式进行设置。

* 设置文档头部

```

title: 相册|脱缰的哈士奇
type: "picture"
top: 100
date: 2017-02-06 15:56:56
categories: Album
tags: Album
```

* 设置图片格式

```
{% raw %}
<style>
.photo img{
  border: 1px solid #999;
  height: 220px;
  width:  260px;
}
.photo li{
    margin: 10px;
    float: left;
    list-style: none;
}
</style>
<div class="photo">
{% endraw %}
## 骑行
* ![骑行](http://ojxk3q6gs.bkt.clouddn.com/my-riding-bike.jpg) 
* ![riding-1](http://ojxk3q6gs.bkt.clouddn.com/riding-1.jpg)
* ![riding-2](http://ojxk3q6gs.bkt.clouddn.com/riding-2.jpg)
* ![riding-3](http://ojxk3q6gs.bkt.clouddn.com/riding-3.jpg)

## 技能
* ![programing](http://ojxk3q6gs.bkt.clouddn.com/programing.jpg)
* ![arm-linux](http://ojxk3q6gs.bkt.clouddn.com/arm-linux.jpg)
{% raw %}

```

## FAQ

## 博客搭建日志
* 2013/08 ~ 2014/03：新浪博客撰写
* 2014/09: CSDN博客和简书博客尝试
* 2015/12: Hexo博客初步搭建，采用Yilia主题
* 2016/04: Hexo插件安装，添加多说评论，RSS订阅，博文置顶等功能
* 2016/11: 更换Next主题，添加搜索，站点统计，主题背景灯功能
* 2017/01: 整理Hexo博客搭建文档，对博文格式进行整理

## 总结
至此，关于使用GitHub和[Hexo](https://hexo.io/)搭建个人博客的系列博文（共3篇）全部整理完毕。

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