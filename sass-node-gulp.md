---
title: sass 2 css gulp workflow
tags:
  - tools
description: '前端基础工作流：sass自动化编辑成css。sass 完整安装过程，配合nodejs ,gulp 工具，实现自动化编译成css。'
date: 2016-05-04 19:34:59
---

目前sass提供了观察文件变化，自动将sass文件编译成css的功能。如：
You can also tell Sass to watch the file and update the CSS every time the Sass file changes:
`sass --watch input.scss:output.css`
If you have a directory with many Sass files, you can also tell Sass to watch the entire directory:
`sass --watch app/sass:public/stylesheets`
在有些机子上跟踪编译非常慢。使用不便。
如果想更顺手的完成更复杂的编译，就需要使用nodejs,gulp 工具进行处理。

[TOC]

### Sass安装
环境ubuntu 16.04
1. `sudo apt-get install ruby`
2. `sudo gem install sass`
<!--more-->
国内会报错，如下
```
sudo gem install sass
ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
Errno::ECONNRESET: Connection reset by peer - SSL_connect (https://api.rubygems.org/quick/Marshal.4.8/sass-3.4.22.gemspec.rz)
```
原因是国内和谐gem，解决方法是使用[淘宝镜像](https://ruby.taobao.org/)如下
```
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***
https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
```
重新执行`sudo gem install sass`

### nodejs安装
安装参考[nodejs.org](https://nodejs.org/en/)
npm访问慢的问题参考[npm.taobao.org淘宝镜像](http://npm.taobao.org/)

### gulp安装
>gulp 介绍
[gulpjs.com](http://gulpjs.com/)
[gulp中文网](http://www.gulpjs.com.cn/)

gulp 安装参考[gulp入门指南](http://www.gulpjs.com.cn/docs/getting-started/)
简易步骤：
在项目更目录执行
`npm install --save-dev gulp`
**安装gulp-sass插件,详细说明*[gulp-sass](https://www.npmjs.com/package/gulp-sass/)
`npm install gulp-sass`

### sass 转 css 操作流
示例项目目录结构
```
-node_modules   //node 模块
-scss           //编译前的scss文件
 --test.scss
-css            //编译后的css文件
 --test.css
--gulpfile.js   //gulp任务执行工具配置文件
--index.html
```
gulpfile.js文件内容
```
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('scss',function(){
	gulp.src('./scss/*.scss')  //这里是scss文件的目录
		.pipe(sass().on('error',sass.logError))
		.pipe(gulp.dest('./css'));  //这里是编译后css存放的目录
})

gulp.task('default',function(){
	gulp.watch('./scss/*.scss',['scss']);  //在这里执行文件观察任务，发现变化执行上面定义好的 `scss`编译任务。
})
```
启动：
`node_modules/.bin/gulp gulpfile.js`
提示
```
[19:05:18] Using gulpfile ~/test/gulpfile.js
[19:05:18] Starting 'default'...
[19:05:18] Finished 'default' after 20 ms
```
打开编辑器编辑scss下test.scss文件，保存，查看css下test.css文件，已经编译好了。
