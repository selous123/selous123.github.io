---
title: Github-pages(Jekyll模板)奇淫巧计
date:  2020-03-14 18:55:13 +0800
category: 杂记
tags: 博客
excerpt: 在github-page中添加元素
---


#### 1. 添加代码块

```
用两个```将代码块包含即可
```

#### 2. 添加图片

##### 2.1. 添加图片并居中显示:
``` 
<center><img src="https://selous123.github.io/assets/img/avatar.webp" width="200" height="auto"/>图片链接不存在</center>
```

<center><img src="https://selous123.github.io/assets/img/avatar.webp" width="200" height="auto"/>图片链接不存在</center>

##### 2.2. 添加其他仓库位置的图片/视频
```
https://raw.githubusercontent.com/用户名/项目名称/分支/图片文件夹/XXX.png
```
例如：
```
https://raw.githubusercontent.com/selous123/denosing-pytorch/master/show/of/loss.png
```

#### 3. 添加视频
##### 3.1. 添加github仓库中视频文件：video 标签
当前主流的方法当然是HTML5中的video标签了，但是
当前，video 元素只支持三种视频格式：

Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件

MPEG4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件

WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件

如果你的视频文件是mp4格式的，那么推荐使用video标签，兼容PC端和移动端。

**代码：**
```
<center>
<video src="https://raw.githubusercontent.com/selous123/denosing-pytorch/master/show/frvdwof/vimeo_presentation.mp4" controls="controls" width="50%" height="auto">
你的浏览器不支持HTML5浏览器
</video>
</center>
```

**效果：**
<center>
<video src="https://raw.githubusercontent.com/selous123/denosing-pytorch/master/show/frvdwof/vimeo_presentation.mp4" controls="controls" width="50%" height="auto">
你的浏览器不支持HTML5浏览器
</video>
</center>

##### 3.2. 从主流网站上添加视频: iframe标签
**代码：**
```
<iframe 
    width="800" 
    height="450" 
    src="https://v.miaopai.com/iframe?scid=SvyHaHOczsp7B6ftW86oqMMz62-h5ai6~Fwp8A__"
    frameborder="0" 
    allowfullscreen>
</iframe>
```

**效果：**
<iframe
    width="800" 
    height="450" 
    src="https://v.miaopai.com/iframe?scid=SvyHaHOczsp7B6ftW86oqMMz62-h5ai6~Fwp8A__"
    frameborder="0" 
    allowfullscreen>
</iframe>



#### 4. 加Tex数学公式支持