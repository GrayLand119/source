---
title: IOS转场动画学习
date: 2016-03-17 11:19:11
tags:
- transition
- objc
categories:
- ios
toc: true 文章目录
---

#IOS转场动画学习

# 视图控制器转换(View Controller Transition)

View Controller Transition 是什么？在 NavigationController 里 push 或 pop 一个 View Controller，在 TabBarController 中切换到其他 View Controller，以 Modal 方式显示另外一个 View Controller，这些都是 View Controller Transition。

视图控制器分2种:
1. UINavigationController，UITabBarController, UISplitController
2. UIViewController

第一种显示内容需要内嵌, 第二种可以直接添加.

转场过程中，作为容器的父 VC 维护着多个子 VC，但在视图结构上，只保留一个子 VC 的视图，所以转场的本质是下一场景(子 VC)的视图替换当前的场景视图(子 VC)以及相应的控制器的切换

<!-- more list -->

目前为止，官方支持以下几种方式的自定义转场：
1. 在 UINavigationController 中 push 和 pop
2. 在 UITabBarController 中切换 Tab
3. Modal 转场：presentation 和 dismissal，俗称视图控制器的模态显示和消失，仅限于modalPresentationStyle属性为 UIModalPresentationFullScreen 或 UIModalPresentationCustom 这两种模式
4. UICollectionViewController 的布局转场：UICollectionViewController 与 UINavigationController 结合的转场方式，实现很简单。


# 定制转场实现方式
转场协议由5种协议组成，在实际中只需要我们提供其中的两个或三个便能实现绝大部分的转场动画.

1. 转场代理(Transition Delegate)
自定义转场的第一步便是提供转场代理，告诉系统使用我们提供的代理而不是系统的默认代理来执行转场。有如下三种转场代理，对应上面三种类型的转场：

```
<UINavigationControllerDelegate> //UINavigationController 的 delegate 属性遵守该协议
<UITabBarControllerDelegate> //UITabBarController 的 delegate 属性该协议
<UIViewControllerTransitioningDelegate> //UIViewController 的 transitioningDelegate 属性遵守该协议
```

2. 动画控制器(Animation Controller)
3. 交互控制器(Interaction Controller)
4. 转场环境(Transition Context)
5. 转场协调器(Transition Coordinator)





