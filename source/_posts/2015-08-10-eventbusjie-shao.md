---
layout: post
title: "eventbus介绍"
date: 2015-05-10 21:37:33 +0800
comments: true
categories: android 
---

![icon](https://github.com/greenrobot/EventBus/raw/master/EventBus-Publish-Subscribe.png "icon")




>EventBus是一款针对Android优化的发布/订阅事件总线。主要功能是替代Intent,Handler,BroadCast在Fragment，Activity，Service，线程之间传递消息.优点是开销小，代码更优雅。以及将发送者和接收者解耦。

###1.下载EventBus的类库

源码：[https://github.com/greenrobot/EventBus](https://github.com/greenrobot/EventBus "url")


<!--more-->

###2.基本使用

1. 自定义一个类，可以是空类，比如：

		public class AnyEventType {  
			public AnyEventType(){}  
		}  
 
2. 在要接收消息的页面注册：

		eventBus.register(this);  

3. 发送消息

		eventBus.post(new AnyEventType event);  

4. 接受消息的页面实现(共有四个函数，各功能不同，这是其中之一，可以选择性的实现，这里先实现一个)：

		public void onEvent(AnyEventType event) {}  

5. 解除注册

		eventBus.unregister(this);  
		
		
详细教程: [http://blog.csdn.net/harvic880925/article/details/40660137](http://blog.csdn.net/harvic880925/article/details/40660137 "url")

