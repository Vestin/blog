---
title: livereload
toc: true
tags:
  - tools
date: 2016-05-06 14:24:57
description: livereload 前端页面保存自动刷新
---

nodejs + chrome extension
1. install [chrome extension - livereload](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei)

2. `sudo npm i livereload -g` 全局安装node插件livereload, [livereload插件详情](https://npm.taobao.org/package/livereload)

3. 在正在修改的html文件中添加
```
<script>
  document.write('<script src="http://' + (location.host || 'localhost').split(':')[0] +
  ':35729/livereload.js?snipver=1"></' + 'script>')
</script>
```
4. 在要修改的文件目录下, 输入命令
```
vestin@vestin:~/test$ livereload
Starting LiveReload v0.4.1 for ~/test/mail.html on port 35729.
```

5. 直接打开html文件，file:///协议就ok

6. 编辑文件，查看结果
