---
title: 如何用最简单的方法搭建自己的github-page
date:  2020-03-14 13:59:13 +0800
category: 杂记
tags: 博客
excerpt: 介绍如何搭建github-page
---

博客最主要的目的是记录自己生活/工作/技术上的点点滴滴，其次才是和大家分享。

### 1. 写在前面
#### 1.1. 如何连接github和电脑git

### 2. 如何搭建自己的github-page.
#### 2.1. 在github网站上新建项目，项目名称为 "用户名.github.io"

<center><img src="https://selous123.github.io/assets/img/blog-gpage/create.png" width="250" height="260"/></center>

#### 2.2. 将该项目pull到本地
> git clone git@github.com:用户名/用户名.github.io.git 

#### 2.3. 选择一个自己喜欢的模板格式，如[我的博客](selous123.github.io),找到该博客的[github repo](https://github.com/selous123/selous123.github.io),然后pull到本地
>git clone https://github.com/selous123/selous123.github.io.git

#### 2.4. 然后将文件下的除了.git外的所有文件复制到自己的目录下

<center><img src="https://selous123.github.io/assets/img/blog-gpage/file.png" width="250" height="260"/></center>

#### 2.5. 将文件push到自己的repo中
> git add -A
> git commit -m "init repo"
> git push origin master


#### 2.6. 最后就可以通过网址: 用户名.github.io访问到博客内容。
<center><img src="https://selous123.github.io/assets/img/blog-gpage/blog.png" width="250" height="260"/></center>

也可以通过github repo的setting选项查看自己blog的状态。

<center><img src="https://selous123.github.io/assets/img/blog-gpage/settings.png" width="250" height="260"/></center>
<center><img src="https://selous123.github.io/assets/img/blog-gpage/gp_zt.png" width="250" height="260"/></center>

### 3. 写在后面
#### 3.1. 如何将博客私有化
#### 3.2. 推荐编辑器--vscode
Enjoy Yourself in Blog!