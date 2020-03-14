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

```
<center>
<video src="https://v.miaopai.com/iframe?scid=SvyHaHOczsp7B6ftW86oqMMz62-h5ai6~Fwp8A__" controls="controls" width="50%" height="auto">
你的浏览器不支持HTML5浏览器
</video>
</center>
```

<center>
<video src="https://v.miaopai.com/iframe?scid=SvyHaHOczsp7B6ftW86oqMMz62-h5ai6~Fwp8A__" controls="controls" width="50%" height="auto">
你的浏览器不支持HTML5浏览器
</video>
</center>



#### 4. 加Tex数学公式支持