---
layout:     post
title:      基于Lnmp集成环境的Laravel5.4环境搭建
subtitle:   
date:       2018-04-20
author:     JKJUN
header-img: img/post-bg-miui6.jpg
catalog: true
tags:
    - Laravel5.4 lnmp
---

>本人做过几个基于Laravel框架的网站项目，例如[电竞招聘网站](http://eshunter.com)在服务器搭建部分遇到很多的坑，现向大家分享一下步骤，也留待我以后查看。

>本次服务器环境配置为：Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-91-generic x86_64)  1核2G 1M带宽 50G存储。

**坑1：服务器内存大小需大于等于2G**

## Lnmp环境搭建
1.  1、下载集成环境lnmp1.4-full.tar.gz。执行命令wget -c http://soft.vpser.net/lnmp/lnmp1.3-full.tar.gz && tar zxf lnmp1.3-full.tar.gz && cd lnmp1.3-full && ./install.sh lnmp

2.  2、解压tar –xvf lnmp1.4-full.tar.gz

3.  3、进入目录，并运行./install.sh

4.  4、	选择mysql版本：理论要求5.6.36以上
![1.png](http://jkjun.cn/images/2018-04-20/1.png)
5.  5、设置mysql初始密码，一定要记住该密码，以后用于登录mysql使用，建议用小本本记下来。小编就因为服务器太多而忘记很多密码，最后不得不重新设置，很麻烦。
![2.png](http://jkjun.cn/images/2018-04-20/2.png)
6.  6、	选择php版本：5.6.31以上，我选择的是5.6.31
![3.png](http://jkjun.cn/images/2018-04-20/3.png)
7.  7、	选择内存分配：默认就行
![4.png](http://jkjun.cn/images/2018-04-20/4.png)
8.  8、	然后开始漫长的等待安装，大概一个小时时间。输入指令 lnmp
![5.png](http://jkjun.cn/images/2018-04-20/5.png)
出现上图所示界面，则表示安装完成。

## 安装配置laravel5.4的环境
```
	安装composer	
	$ curl -sS https://getcomposer.org/installer | php 
	$ mv composer.phar /usr/local/bin/composer #使用国内镜像 
	$ composer config -g repo.packagist composer https://packagist.phpcomposer.com 
	$ composer -v 
```
逐行执行上述命令，得到的结果如下图，表明composer已安装成功。
![6.png](http://jkjun.cn/images/2018-04-20/6.jpg)

使用composer 创建 laravel 项目
```
	composer create-project --prefer-dist laravel/laravel blog "5.4.*"
```
或者直接将你的项目克隆到网站目录下

**坑2：网站打开一片空白-无任何报错信息**
```
	解决办法有以下几个部分：
	1、检查是否开启了Lravel的debug调试，在.env文件中开启debug
	2、检查是否开启php配置文件中debug打印功能，在php.ini配置文件中。
	3、可能是没有开启网站访问路径限制为根目录上一层，因为laravel的入口地址在public文件下，所有整个网站的访问权限路径应该在public上层。修改文件/user/local/nginx/conf/fastcgi.conf  将open_basedir=/home/wwwroot/demo/public:/tmp/:/proc/";改为open_basedir=/home/wwwroot/demo/:/tmp/:/proc/";
```

**坑3：网站访问权限不够**
```
	给storage目录及public目录赋予权限。chmod 777 storage
```
