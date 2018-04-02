---
layout:     post
title:      Centos 搭建pppoe环境
subtitle:   
date:       2018-04-02
author:     JKJUN
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Centos
    - pppoe
---

>最近公司有用到分析pppoe协议，所以在本地虚拟机环境搭建了一下pppoe的本地测试环境，记下整个流程，以便自己记忆

>本地环境为centos6.5  配置两块网卡，eth0 192.168.0.175 网关192.168.0.1 为外网口  eth1 192.168.2.100 内网口

## 安装pppoe
```
    yum install rp-pppoe
    #安装完成后系统 使用 pppoe-server 命令来进行管理控制
```

## 安装完成后，目录/etc/ppp下会生成pppoe-server-options文件，修改如下：
```
	# PPP options for the PPPoE server
    # LIC: GPL
    require-pap
    require-chap
    login
    lcp-echo-interval 10
    lcp-echo-failure 2
    logfile /var/log/pppoe.log
```

## 添加用户名密码
#### 修改文件/etc/chap-secrets文件
```
	#添加一行
    pppoe * "123456" *
    #意思是新增一个用户，用户名是pppoe 密码是123456 用于登录pppoe服务
```

##启动pppoe
```
	pppoe-server -I eth0 -L 192.168.26.1 -R 192.168.26.10 -N 10
    #-I eth0  在eth0端口上检测pppoe discover包
    #-L 192.168.26.1    虚拟网关的意思，就是pppoe服务器端虚拟网关ip
    #-R 192.168.26.10   分配虚拟IP，从ip192.168.26.10开始
    #-N 10              分配地址总个数
    #还有一些其他的参数你可以参考一下，直接man pppoe-server自己看了，每个参数都有默认值
```

##添加防火墙规则
```
	#禁止pppoe ip 对本机的访问
    iptables -A INPUT -i eth0 -s 192.168.26.0/24 -j DROP
    #添加nat转换
    iptables -t nat -A POSTROUTING -s 192.168.26.0/24 -j SNAT --to-source 192.168.0.175
    #开启路由转发
    echo 1 >/proc/sys/net/ipv4/ip_forward
```
    
##测试连接 局域网内添加pppoe拨号连接
  测试成功
  
##以下是我目前使用的脚本代码
```
    1 #!/bin/sh
    2 ifconfig eth0 192.168.0.175 netmask 255.255.255.0
    3 route add default gw 192.168.0.1
    4 killall pppoe-server
    5 pppoe-server -I eth1 -L 192.168.26.1 -R 192.168.26.10 -N 10
    6 iptables -F
    7 echo 0 >/proc/sys/net/ipv4/ip_forward
    8 iptables -A INPUT -i eth1 -s 192.168.26.0/24 -j DROP
    9 iptables -t nat -A POSTROUTING -s 192.168.26.0/24 -j SNAT --to-source 192.168.0.175
    10 echo 1 >/proc/sys/net/ipv4/ip_forward
```