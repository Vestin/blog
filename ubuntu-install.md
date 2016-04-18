---
title: ubuntu 系统安装后必做
date: 2016-04-18 13:30:36
description: ubuntu 系统安装后必做
tags:
- ubuntu
categories: 
- linux
toc: true
---
ubuntu 安装
===
2015-10-14 14:46:28 星期三 @vestin

### 系统安装
1. 下载 image writer 镜像写入工具
2. 下载 ubuntu 最新系统 [ubuntu 下载](http://cn.ubuntu.com/download "ubuntu 下载")
3. 写入镜像 安装

<!--more-->

### 系统设置
* root 用户添加
	* sudo passwd root
	* su
* 外观
	* 主题：Radiance
	* 行为：开启工作区
* 软件和更新
	* 附加驱动：使用附加驱动
	* 更新：每周
* 时间和日期
	* 时钟：星期，日期，年份，月历
* 用户账户
	* 在菜单栏显示登录名
* 拒绝GUEST帐号登录
	* ```
	sudo sh -c 'printf "[SeatDefaults]\nallow-guest=false\n" >/usr/share/lightdm/lightdm.conf.d/50-no-guest.conf'
	```
* 链接到windows的``WORKGROUP``
	* gedit /etc/samba/smb.conf
		workgroup = WORKGROUP
		netbios name = something
	* 文件管理器``CRTL+L`` 输入 ``smb://xxx.xxx.xxx.xxx/`` 链接

### 工具安装
* su
* filezilla
	* apt-get install filezilla
* vim
	* apt-get install vim
* 天涯vpn配置
	* 网络链接->vpn->配置vpn
	网关:``us5.tyvpn.cn``
	高级:身份验证(MSCHAP,MSCHAP2) 安全(MMPE)
* chorme 登录
* [shadowsocks-qt5](https://github.com/librehat/shadowsocks-qt5 "shadowsocks-qt5")
	* ```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5 
	```
	* qt5 配置
		* 配置
服务器IP:
运营部可使用以下两个(建议使用不同的)
173.254.200.25
173.254.200.26
技术部可以使用以下三个
173.254.200.34
173.254.200.25
173.254.200.18
密码:Ya*******
加密:aes-256-cfb(默认，无需改动)
备注:无关紧要，自己填写
代理端口:1080(默认，无需改动)

* 邮件客户端thunderbird 配置
	* 网易企业客户端配置
接受 imap.ym.163.com 993 ssl/tls 普通密码
发送 smtp.ym.163.com 465 ssl/tls   普通密码
* phpstorm 安装
	* apt-get install default-jre
	* 下载[phpstorm9.0](http://download-cf.jetbrains.com/webide/PhpStorm-9.0.tar.gz) 到 /myfile
	* 解压，移动到 /usr/share ,创建软链接到/usr/bin 中
	* 启动，锁定到任务栏
	* User Name: newasp
	```
===== LICENSE BEGIN =====
14617-12042010
00001xrVkhnPuM!Bd!vYtgydcusnqt
mM!hZWoGg"DprWxZCBwsy8T91O7MRu
NVHtrbzv8O9mmoLvtijcHSSE7i5Jr!
===== LICENSE END =====```
* sublime 安装配置
	* [sublime3](http://www.sublimetext.com/3)
	* dpkg -i .deb
	* 配置
	
* unity tool 工具设置
	* sudo  apt-get  install  unity-tweak-tool
	* dash 中打开unity-tweak-tool
* conky 系统监视
	* sudo apt-get install conky
	
### 工作环境安装
* apt-get install php5
* apt-get install mysql-server
* apt-get install mysql-client
* apt-get install php5-mysql
* apt-get install php5-xdebug
* apache2 配置
* php5 配置
* 安装 git


5 axel、aria 多线程下载。
axel -n 10 下载url， 开10线程下载文件
百度云的东西可以用axel下载，我经常在服务器用axel下载百度云的东西。

**[Top Things To Do After Installing Ubuntu 15.04](http://www.unixmen.com/top-things-to-do-after-installing-ubuntu-15-04/)**