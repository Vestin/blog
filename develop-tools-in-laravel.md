---
layout: blog
title: Larvel 开发中使用顺手的一些工具集
date: 2017-10-25 15:34:54
tags:
- tools
- laravel
categories:
- coding
---

开发项目较多，使用到很多工具，记录下来，方便使用。

**Larvel 开发中使用顺手的一些工具集**
* ide-helper

<!--more-->

### ide-helper
`Laravel`中大量使用`facade`,使IDE识别困难,配合`Phpstorm`的`Laravel Plugin`插件使用，效果完美。
* laravel依赖包 [ide-helper](https://github.com/barryvdh/laravel-ide-helper)
* Phpstorm插件 [laravel Plugin](https://github.com/Haehnchen/idea-php-laravel-plugin)

常用命令
```
//生成辅助文件
php artisan ide-helper:generate

//生成model的phpdoc
php artisan ide-helper:models
```