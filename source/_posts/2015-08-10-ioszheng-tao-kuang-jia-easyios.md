---
layout: post
title: "ios整套框架easyios"
date: 2015-04-10 22:10:43 +0800
comments: true
categories: ios
---


###EasyIOS 以提升开发效率为宗旨
---

* 代码分离 -Model-View-ViewModel- 分离ViewController中的大量逻辑代码，解决ViewController承担了过多角色而造成的代码质量低下。增加视图与模型的绑定特性。

* 自动持久化 -Model to Db– 我再也不想思考如何实现持久化了。在我的想法里，将模型对象直接扔到一个bucket里，然后它就能自动的对数据进行存储、缓存、合并以及唯一化。我应当关注于描述对象间的属性和联系，以及我希望它们分组的方式。其他的实现细节都应该是不可见的。

* 自动RESTful API –Json to Model- 一旦我给程序发出指令，将一个API响应对应到一个数据对象，网络和JSON转换应该被自动完成。我只想关注如何将JSON中那些项目展示给用户。

* 有表现力的触发器和响应 -ReactiveCocoa– 我想用源于响应意图（Intent）的语法来描述事件的响应和触发器，我不关心它们间的连接是如何实现的，并且这些连接也不应该在重构时出错。

* 简洁明了的网络请求 -Action and Request- 对于简单的GET、POST请求，可以进行对象化操作，我只想告诉程序，链接在哪里，有哪些参数，接下来就自动拉取到想要的数据，顺便帮我把缓存也做齐了，也是极好的。

* 便捷的UI布局 – FLKAutolayout-更加便捷的进行autolayout布局,不管你使用springs & struts或者AutoLayout，每种方法都需要你明确相关视图如何排列。你需要花大量的时间编写和修正这些排列，特别是现在有这么多设备需要适配 的情况下。没有什么是自动写好的，UI布局依赖于对细节的不断调整。推荐开发期间Debug工具FLEX,pod 'FLEX', '~> 1.1.1'需要手动集成，发布release版本时请删除。

* 友好的线程控制 -GCDObjC-
* 便捷的正则匹配
* 富文本的Label
* and so on……

<!--more-->

###The MVVM(Model-View-ViewModel)
---
全新基于MVVM(Model-View-ViewModel)编程模式架构，开启EasyIOS开发函数式编程新篇章。

EasyIOS 2.0类似AngularJs，最为核心的是：MVVM、ORM、模块化、自动化双向数据绑定、等等

喜欢swift的同学，同样有swift的2.0 demo [RACSwift for EasyIOS](https://github.com/zhuchaowe/RACSwift "Title")，供大家学习。

关于有疑问什么是MVVM，以及为什么IOS开发需要MVVM思想编程的，请看文章用[Model-View-ViewModel构建iOS App](http://easyios.08dream.com/index.php?s=/Home/Article/detail/id/10036.html "Title")有详细介绍.

EasyIOS 2.0是基于MVVM编程思想进行构建的，封装了Scene,SceneModel,Model，Action四种模型来对IOS进行开发，4种模型的定义解决了IOS开发中ViewController承担了过多角色而造成的代码质量低下，使得结构思路更加清晰。

1. 其中Scene就是ViewController的子类，仅仅负责界面的展示逻辑
2. Model数据模型，父类实现了ORM，可以实现json、object、sqlite三者之间的一键转换,
3. SceneModel 视图-数据模型，主要负责 视图与模型的绑定工作，其中binding的工作交给了ReactiveCocoa。
4. SceneModel包含Action成员，Action类主要负责网络数据的请求,数据缓存，数据解析工作

如果你有看Github的Trending Objective-C榜单，那你肯定是见过ReactiveCocoa了。如果你在微博上关注唐巧、onevcat等国内开发者。那也应该听说过ReactiveCocoa了。

* ReactiveCocoa简称RAC，就是基于响应式编程思想的Objective-C实践，它是Github的一个开源项目，你可以在[这里](https://github.com/ReactiveCocoa/ReactiveCocoa "Title")找到它。

* 二次封装AFNetworking，集成到Action，增加了网络缓存功能，轻松控制是否启用缓存。

* 采用ReactiveCocoa 框架，实现响应式编程，减少代码复杂度。

* Model类整合JsonModel的类库和MojoDataBase类库

* 整合了很多开源的优秀代码

###常用类库：
---

* Action 负责网络数据请求

* Model 负责数据存储

* SceneModel 负责Scene与Model的绑定，调用action进行数据请求

* Scene 一个视图相当于UIViewController,提供了快速集成网络请求和下拉刷新上拉加载的方法。

* SceneTableView 一个TableView，配合Scene提供了集成下拉刷新上拉加载的方法

* SceneCollectionView 一个CollectionView，配合Scene提供了集成下拉刷新上拉加载的方法


github地址: [https://github.com/zhuchaowe/EasyIOS](https://github.com/zhuchaowe/EasyIOS "Title")