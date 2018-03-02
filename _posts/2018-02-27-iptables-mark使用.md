---
layout:     post
title:      iptables mark 使用
subtitle:   整理iptables mark 使用命令
date:       2018-02-27
author:     JKJUN
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iptables
    - MARK
---

>最近在使用iptables 的mark模块，为防止以后忘记命令使用，故写此博客。

# 所有TCP数据标记1
	iptables -t mangle -A PREROUTING -p tcp -j MARK --set-mark 1
	
# 匹配标记1的数据并保存数据包中的MARK到连接中
	iptables -t mangle -A PREROUTING -p tcp -m mark --mark 1 -j CONNMARK --save-mark
	
# 标记连接
	iptables -t mangle -A PREROUTING -p tcp -j CONNMARK --set-mark 1
	
# 匹配连接标记1并将连接中的标记设置到数据包中
	iptables -t mangle -A PREROUTING -p tcp -m connmark --mark 1 -j CONNMARK --restore-mark
	
# 接下来就在forward链进行匹配数据包。
	iptables -t mangle -A PREROUTING -p tcp -m mark --mark 1 -j DROP
