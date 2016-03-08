---
title: KVO KVC 学习
date: 2016-01-16 13:58:41
tags:
- objc
categories:
- ios
toc: true 文章目录
---
 
# KVO KVC 简述

<!-- more -->

# 1.KVC
KVC，即是指 **NSKeyValueCoding**，一个非正式的 Protocol，提供一种机制来间接访问对象的属性。KVO 就是基于 KVC 实现的关键技术之一。

一个对象拥有某些属性。比如说，一个 Person 对象有一个 name 和一个 address 属性。以 KVC 说法，Person 对象分别有一个 value 对应他的 name 和 address 的 key。 key 只是一个字符串，它对应的值可以是任意类型的对象。从最基础的层次上看，KVC 有两个方法：一个是设置 key 的值，另一个是获取 key 的值。如下面的例子：

```objc
void changeName(Person *p, NSString *newName)
{

// using the KVC accessor (getter) method
NSString *originalName = [p valueForKey:@"name"];

// using the KVC  accessor (setter) method.
[p setValue:newName forKey:@"name"];

NSLog(@"Changed %@'s name to: %@", originalName, newName);

}
```

现在，如果 Person 有另外一个 key 配偶（spouse），spouse 的 key 值是另一个 Person 对象，用 KVC 可以这样写：
```objc
void logMarriage(Person *p)
{

// just using the accessor again, same as example above
NSString *personsName = [p valueForKey:@"name"];

// this line is different, because it is using
// a "key path" instead of a normal "key"
NSString *spousesName = [p valueForKeyPath:@"spouse.name"];

NSLog(@"%@ is happily married to %@", personsName, spousesName);

}
```

key 与 key pat 要区分开来，key 可以从一个对象中获取值，而 key path 可以将多个 key 用点号 “.” 分割连接起来，比如：

> [p valueForKeyPath:@"spouse.name"];

相当于这样……

> [[p valueForKey:@"spouse"] valueForKey:@"name"];


# 2.KVO
Key Value Observer , 其实就是一种**观察者模式**.  被监听对象状态改变时会主动通知观察者.

* 使用方法

1. 注册与解除注册

```objc
// 注册与解除注册一定是配对使用,否则或导致资源泄露.
// 注册
- (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(void *)context;
// 解除注册
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;
```
2. 设置属性
只有遵循KVO的方式设置属性,观察者才会获取通知.即使用**setter**方法或通过**key-path**来设置.
```objc
UIView *view;

[view setFrame:CGRectZero];
[view setValue:CGRectZero forKey:@"frame"];
```
3. 处理变更通知
**某一对象**添加**观察者**后, 对其进行了**设置属性**,系统会调用以下代理方法.
实现此代理,并在其中处理变更通知
```objc
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context;
```

详细使用见以下链接:
[详解键值观察（KVO）及其实现机理](http://blog.csdn.net/kesalin/article/details/8194240)

