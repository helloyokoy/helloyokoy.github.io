---
layout: post
title: "ios依赖注入框架objection"
date: 2015-07-10 21:52:44 +0800
comments: true
categories: ios
---


>objection 是一个轻量级的依赖注入框架，受Guice的启发，Google Wallet 也是使用的该项目。「依赖注入」是面向对象编程的一种设计模式，用来减少代码之间的耦合度。通常基于接口来实现，也就是说不需要new一个对象，而是通过相关的控制器来获取对象。2013年最火的PHP框架 laravel 就是其中的典型。


假设有以下场景：ViewControllerA.view里有一个button，点击之后push一个ViewControllerB，最简单的写法类似这样：


	- (void)viewDidLoad
	{
    	[super viewDidLoad];
    	UIButton *button = [UIButton buttonWithType:UIButtonTypeSystem];
    	button.frame = CGRectMake(100, 100, 100, 30);
    	[self.view addSubview:button];
	}
 
	- (void)buttonTapped
	{
    	ViewControllerB *vc = [[ViewControllerB alloc] init];
    	[self.navigationController pushViewController:vc animated:YES];
	}
 

这样写的一个问题是，ViewControllerA需要import ViewControllerB，也就是对ViewControllerB产生了依赖。依赖的东西越多，维护起来就越麻烦，也容易出现循环依赖的问题，而objection正好可以处理这些问题。

<!--more-->

实现方法是：先定义一个协议(protocol)，然后通过objection来注册这个协议对应的class，需要的时候，可以获取该协议对应的object。对于使用方无需关心到底使用的是哪个Class，反正该有的方法、属性都有了(在协议中指定)。这样就去除了对某个特定Class的依赖。也就是通常所说的「面向接口编程」。


	JSObjectionInjector *injector = [JSObjection defaultInjector]; // [1]
	UIViewController <ViewControllerAProtocol> *vc = [injector getObject:@protocol(ViewControllerAProtocol)]; // [2]
	vc.backgroundColor = [UIColor lightGrayColor]; // [3]
	UINavigationController *nc = [[UINavigationController alloc] initWithRootViewController:vc];
	self.window.rootViewController = nc;
	
1. 获取默认的injector，这个injector已经注册过ViewControllerAProtocol了。
2. 获取ViewControllerAProtocol对应的Object。
3. 拿到VC后，设置它的某些属性，比如这里的backgroundColor，因为在ViewControllerAProtocol里有定义这个属性，所以不会有warning。

可以看到这里没有引用ViewControllerA。再来看看这个ViewControllerAProtocol是如何注册到injector中的，这里涉及到了Module，对Protocol的注册都是在Module中完成的。Module只要继承JSObjectionModule这个Class即可。


	@interface ViewControllerAModule : JSObjectionModule
	@end
 
	@implementation ViewControllerAModule
	- (void)configure
	{
    	[self bindClass:[ViewControllerA class] toProtocol:@protocol(ViewControllerAProtocol)];
	}
	@end
 

绑定操作是在configure方法里进行的，这个方法在被添加到injector里时会被自动触发。


	JSObjectionInjector *injector = [JSObjection defaultInjector]; // [1]
	injector = injector ? : [JSObjection createInjector]; // [2]
	injector = [injector withModule:[[ViewControllerAModule alloc] init]]; // [3]
	[JSObjection setDefaultInjector:injector]; // [4]
 

1. 获取默认的 injector
2. 如果默认的 injector 不存在，就新建一个
3. 往这个 injector 里注册我们的 Module
4. 设置该 injector 为默认的 injector

这段代码可以直接放到 + (void)load里执行，这样就可以避免在AppDelegate里import各种Module。

因为我们无法直接获得对应的Class，所以必须要在协议里定义好对外暴露的方法和属性，然后该Class也要实现该协议。


	@protocol ViewControllerAProtocol <NSObject>
	@property (nonatomic) NSUInteger currentIndex;
	@property (nonatomic) UIColor *backgroundColor;
	@end
 
	@interface ViewControllerA : UIViewController <ViewControllerAProtocol>
	@end
 

通过objection实现依赖注入后，就能更好地实现SRP(Single Responsibility Principle)，代码更简洁，心情更舒畅，生活更美好。


github地址: [https://github.com/atomicobject/objection](https://github.com/atomicobject/objection "url")

demo地址: [https://github.com/helloyokoy/bizhi](https://github.com/helloyokoy/bizhi "url")