---
layout: post
title: iOS AFNetworking
category: 问题
tags: [疑难问题]
---

## problem

最近写一个http请求，调了快3天才出来，表示心里的那个什么马啊。。。真是在心中不停的冲过来。


## 具体问题

因为需要一个http协议来返回一些资料。。这里后台采用了Yii框架来写。。当吭哧吭哧配好环境，跑起来。。。写好controller和action.
本以为一切水道渠成，没想到竟然afnetwoking报错咯。。。。

JSON text did not start with array or object and option to allow fragments not set。。

因为以前写这个也没啥问题，很容易想到难道是Yii的问题。于是手动写了个php的页面，跑起来。。竟然木有问题。。。这肯定是Yii的锅咯。。于是开始了漫长的Yii框架问题查找。。修改Content-type, AFNetwoking请求的serializer修改。。。。继续错误中。。。
看来需要使用出杀手锏。。抓包。。 当以为一切事情会顺利的时候。。。才发现一直改到两个url的回包完全一样的时候，竟然还是一个是好的，一个错误，完全崩溃中。。。



崩溃的年代，需要一个英雄。想成英雄，毕竟要有天时地利人和。。。。积攒了一天的人品，也需要爆发一下咯。。。。才发现原来是这么一个问题。。
自己写的页面是xx/index.php。。 而Yii的路由规则，写了个xx/index.php？r=xx 这种。 而用afnetworking请求用的是post请求。 于是我们换成get请求，英雄诞生咯，世界和平咯。 问题解决咯。。。竟然花了快3天时间，没想到是这个问题。



