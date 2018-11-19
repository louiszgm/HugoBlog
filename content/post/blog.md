+++
title = "Hello World"
lastmod = 2018-11-19T10:41:25+08:00
draft = false
+++

## Hello World! {#hello-world}

This is my First Blog!

Written by [Org-Mode](https://orgmode.org/) in `Emacs`.

Used [org2nikola](https://github.com/redguardtoo/org2nikola) to Export Html.

Powered by [Nikola](https://getnikola.com/).


## 管理docker脚本 {#管理docker脚本}


### org-mode {#org-mode}

通过emacs的org-mode来管理docker的脚本有以下好处:

-   不用在多个脚本文件中来回切换，可以在一个文件中统一管理


## org-mode语法练习 {#org-mode语法练习}


### Lord of the Rings {#lord-of-the-rings}

My favorite scenes are (in this order)

1.  The attack of the Rohirrim
2.  Eowyn's fight with the witch king
    -   this was already my favorite scene in the book
    -   I really like Miranda Otto.
3.  Peter Jackson being shot by Legolas

    -   on DVD only

    He makes a really funny face when it happens.

But in the end, no individual scenes matter but the film as a whole.
Important actors in this film are:

Elijah Wood
: He plays Frodo

Sean Astin
: He plays Sam, Frodo's friend.  I still remember
    him very well from his role as Mikey Walsh in The Goonies.


## cocoscreator 自动化构建iOS工程 {#cocoscreator-自动化构建ios工程}

所谓的自动化构建，最理想的情况应该是在项目build出来之后，不用手动打开 `xcodeproj` 工程去配置就可以直接打包成功并且分发到对应需要的渠道。

在用 [fastlane](https://fastlane.tools/) 进行 `iOS` 自动化构建的时候，会遇到下面的这几个问题：

1.  xcodeproj文件版本太老
2.  项目签名问题
3.  build号自增问题
4.  自动将工程修改为手动签名
5.  使用 weak reference 时，在编译的时候报这个错误， **Cannot synthesize weak property in file using manual reference counting**

, 解决方案： 在 build setting中把 Weak References in Manual Retain Release 设置为 Yes


## docker In docker {#docker-in-docker}

Remove container

sudo docker container rm $(sudo docker ps -f status=exited -q)

config docker.service in Ubuntu16.04

refer link: <https://medium.com/@sudarakayasindu/enabling-and-accessing-docker-engine-api-on-a-remote-docker-host-on-ubuntu-16-04-2c15f55f5d39>

Ubuntu16.04 start docker service through systemctl

sudo systemctl daemon-reload

sudo systemctl restart docker

config docker.service path: /lib/systemd//system/docker.service

capath = ~/ca

exec follow cli to test the remote api:

sudo docker --tlsverify --tlscacert=../ca.pem --tlscert=cert.pem --tlskey=key.pem -H=127.0.0.1:2375 version


## 写博客也能DevOps {#写博客也能devops}

涉及到了如下的技术点:

1.  emacs
2.  org-mode
3.  docker
4.  jenkins

最终达到的目的就是，在emacs中写好文章，然后trigger一个动作，就完成博客的发布。不用脱离emacs


### docker {#docker}

1.  docker cli. docker-compose
2.  docker remote api, use in safe way (autho)
3.  multiple container, access network


## 集成room注意事项 {#集成room注意事项}

最近在集成 android-jetpack的组件room的时候，遇到了一些问题，记录一些。

根本原因：
项目本身的依赖的support包版本过低，而room依赖的support包的版本是 26.1.0，

最终在编译的时候用的是26.1.0的版本，而这个版本里面会删除一些低版本support包的一些类。
比如：

android - java.lang.NoClassDefFoundError: Failed resolution of: Landroid/support/v4/animation/AnimatorCompatHelper
