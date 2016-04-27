---
title: google fonts 国内使用解决方案
toc: true
categories:
  - front-end
tags:
  - front-end
date: 2016-04-26 10:47:01
description: google fonts 离线使用
---

由于众所周知的原因，国内使用google font库有很大的问题。

解决方案1：使用国内镜像如[360网站卫士常用前端公共库CDN服务](http://libs.useso.com)
* 优点：使用方便
* 缺点：目标用户包含国外的开发者，不清楚国外用户的加载速度

解决方案2：提供另外一种解决方案，可以自主决定资源下载源，自主配置cdn等服务。

1. 在[google fonts 官网](https://www.google.com/fonts/)上选择字体并获取css链接，如下
```
<link href='https://fonts.googleapis.com/css?family=Oswald' rel='stylesheet' type='text/css'>
```
2. 将链接内容下载到本地保存，打开，内容如下：
```
/* latin */
@font-face {
  font-family: 'Oswald';
  font-style: normal;
  font-weight: 400;
  src: local('Oswald Regular'), 
       local('Oswald-Regular'), 
       url(https://fonts.gstatic.com/s/oswald/v10/pEobIV_lL25TKBpqVI_a2w.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2212, U+2215, U+E0FF, U+EFFD, U+F000;
}
/* 如下可能还有更多代码，但结构是和上面的一样的 */
```
3. 将 @font-face 下 src属性下 url 处的文件下载到本地并保存，并将 url 地址修改成本地地址
4. 引用修改后的本地google fonts css文件,就可以使用了。

参考资料：
* https://www.zhihu.com/question/19578734
* https://www.google.com/fonts/