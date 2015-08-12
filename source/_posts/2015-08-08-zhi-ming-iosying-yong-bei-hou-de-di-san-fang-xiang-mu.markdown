---
layout: post
title: "知名ios应用背后的第三方项目"
date: 2014-10-28 22:17:44 +0800
comments: true
categories: ios
---
>知名应用程序的设计和技术一直都是开发者需要学习的，同样这些应用所使用的开源框架也是不可忽视的一部分。此前《iOS第三方开源库的吐槽和备忘》中作者ibireme列举了国内多款知名应用所使用的开源框架，并对其中一些框架进行了分析，同样国外开发者@iOSCowboy也在博客中给我们列出了国外多款知名应用使用的开源框架。另外txx's blog中详细介绍了Facebook Paper使用的第三方库。 
 
###Instagram
---
_AFNetworking: 适用于iOS和OS X的网络框架。_

Appirater: 提醒用户打分。

ASIHTTPRequest：简单使用CFNetwork API封装进行HTTP网络请求，用Objective-C编写，可应用在Mac OSX和iOS开发中。

CocoaHTTPServer: 用于Mac OS X和iOS应用程序的轻量级、可嵌入的HTTP服务器框架。

_Cocoa Lumberjack:适用于Mac和iOS的日志框架，集简单、快速、强大以及灵活于一身。_

_MBProgressHUD: 用多种样式展示半透明的HUD，并带有指示器和标签，自定义功能强大。_

PLCrashReporter (Github mirror): 进程内崩溃报告框架。

QSUtilities: 实用工具、控件以及其他辅助类的集合。

SocketRocket: Objective-C WebSocket客户端库。 https://github.com/square/SocketRocket

_XBImageFilters:允许实时过滤摄像头拍摄的照片，使用OpenGL ES 2 来快速处理各种图片效果。_
 
 <!--more-->
 
###Foursquare 
---
Facebook SDK for iOS: 集成Facebook,构建强大的社交app。

FSNetworking: Foursquare iOS网络库。

kingpin: MapKit/MKAnnotation pin 聚合库，主要用来在地图上面添加锚点。

_AFNetworking:适用于iOS和OS X的网络框架。_

SKBounceAnimation: CAKeyframeAnimation子类，可快速简单地设置弹动的数量，开始和结束的值，以及创建动画。 

DB5: 通过Plist配置文件。
 
###LinkedIn
---
_BlocksKit: blocks工具包。_

_SDWebImage: 提供一个UIImageVIew类以支持远程加载网络图片。具有缓存管理、异步图片下载等功能，支持GIF动画，使用GCD和ARC。_

DTCOreText:文字效果代码类库。在UITextView上实现丰富的文字效果，比如文字大小、颜色、字体、下划线，链接，给文字加上图片、视频，文字任意间距等等。实现类似于CSS网页的文字效果。
 
###Shazam
---
AudioStreamer:Mac OS X和iPhone上适用的流媒体音频播放器，可播放来自网络上的音乐。

ColorArt: iTunes 11风格的颜色匹配代码。

objc-geohash: Objective-C GeoHash库，通过经纬度获得哈希表。

FormatterKit: 收集了精心构思的NSFormatter子类。

UIView+Glow: UIView的一个类别，可添加对制作发光视图的支持，以突出屏幕上重要的部分，方便用户与之进行交互。

_WEbViewJavascriptBridge: 在使用UIWebView时，它优雅地实现了JS与ios 的ObjC 原生代码之间的互调，支持消息发送、接收、消息处理器的注册与调用以及设置消息处理的回调。_
 
###Skype
---
_AFNetworking: 适用于iOS和OS X的网络框架。_

Hockey SDK: HockeyApp service官方iOS SDK。

PLCrashReporter (Github mirror): 进程内的崩溃报告框架。

_TTTAttributedLabel是一个文字视图开源组件，是UILabel的替代元件，可以以简单的方式展现渲染的属性字符串。另外，还支持链接植入，不管是手动还是使用UIDataDetectorTypes自动把电话号码、事件、地址以及其他信息变成链接。_

_SDWebImage: 提供一个UIImageVIew类以支持远程加载网络图片。具有缓存管理、异步图片下载等功能，支持GIF动画，使用GCD和ARC。_

_Cocoa Lumberjack: 适用于Mac和iOS的日志框架，集简单、快速、强大以及灵活于一身。_

MWPhotoBrowser: 一个简单的带有栅格视图的iOS照片浏览器，可添加标题和选择多个图片。照片浏览器效果类似iOS原生的照片应用,可显示来自手机的图片或者是网络图片，也可自动从网络下载图片并进行缓存，还可图片进行缩放等。

BlocksKit: Objective-C blocks工具包。
 
###Spotify
---
_FMDB: SQLite API封装库。_

MAObjCRuntime:将运行时API封装成ObjC。

Nu: 编程语言。

PLCrashReporter (Github mirror):进程内崩溃报告框架。

SBJSON:Objective-C 实现的一个严格的JSON 解析器和生成器。