---
layout:     post
title:      .gitignore 问题
subtitle:   
date:       2018-08-22
author:     JKJUN
header-img: img/post-bg-miui6.jpg
catalog: true
tags:
    - gitignore
---

# 背景
 ### Git中有一个非常重要的一个文件-----.gitignore 今天给大家免费送一个.gitignore忽略文件配置清单。大家一定要养成在项目开始就创建.gitignore文件的习惯，否则一旦push，处理起来会非常麻烦。


# 当然如果已经push了怎么办?当然也有解决方法，如下：

有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

git rm -r --cached .

git add .

git commit -m 'update .gitignore'

?



# 创建命令
在版本管理的根目录下（与.Git文件夹同级）创建一个 .gitignore（gitignore是隐藏文件，所以前面有个点）

创建命令：gitignore - Specifies intentionally untracked files to ignore

首先要强调一点，这个文件的完整文件名就是“.gitignore”，注意最前面有个“.”。这样没有扩展名的文件在Windows下不太好创建，这里给出win7的创建方法：创建一个文件，文件名为：“.gitignore.”，注意前后都有一个点。保存之后系统会自动重命名为“.gitignore”。一般来说每个Git项目中都需要一个“.gitignore”文件，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。实际项目中，很多文件都是不需要版本管理的，比如Python的.pyc文件和一些包含密码的配置文件等等。

应用实例(摘自互联网)

项目中有clist.h clist.c main.c三个文件，编译执行后，生成了三个文件 clist.o main.o main(执行文件)。这三个文件是不需要进行版本管理的，所以需要忽略这些文件，使用 git stauts查看后，发现这三个文件也是处于 Untracked files状态。而实际上我们是想忽略他。

使用gitignore文件来解决这个问题，步骤是：

[plain] view plain copy

S1: touch .gitignore #创建gitignore隱藏文件

S2: vim .gitignore #编辑文件，加入指定文件

### 下面是我的gitignore文件的内容

#### 忽略gitignore文件

.gitignore

#### 忽略后缀名为.o和.a的文件

*.[oa]

#### 显示指定忽略名称为main的文件
main
文件.gitignore的格式规范：

A：#为注释

B：可以使用shell所使用的正则表达式来进行模式匹配

C：匹配模式最后跟"/"说明要忽略的是目录

D：使用！取反（例如目录中包含 test.a，并且gitignore文件中包含 *.[oa]，如果在文件中加入 ！test.a 表明忽略除test.a文件以外的后缀名为.a或者.o的文件）

配置完.gitignore文件后，执行git status命令，会发现那三个文件不再是Untracked files了，也就完成了忽略指定文件的功能。

生产配置大奉送

栗子

# 此为注释 – 将被 Git 忽略

*.a # 忽略所有 .a 结尾的文件

!lib.a # 但 lib.a 除外

/TODO # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO

build/ # 忽略 build/ 目录下的所有文件

doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt

.gitignore最强配置清单 如下：

/gradle/wrapper/gradle-wrapper.properties

##----------Android----------

# build

*.apk

*.ap_

*.dex

*.class

bin/

gen/

build/

# gradle

.gradle/

gradle-app.setting

!gradle-wrapper.jar

build/

local.properties

##----------idea----------

*.iml

.idea/

*.ipr

*.iws

# Android Studio Navigation editor temp files

.navigation/

##----------Other----------

# osx

*~

.DS_Store

gradle.properties