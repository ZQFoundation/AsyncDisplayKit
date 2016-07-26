---
title: Subclassing 
layout: docs
permalink: /docs/subclassing.html
prevPage: containers-overview.html
nextPage: faq.html
---

当创建一个subclass时，一个最重要的区别就是这个subclass是继承与ASViewController还是ASDisplayNode。这听起来很明显，但由于其中的一些差异是微妙的，所以铭记于心是非常重要的
## ASDisplayNode
<br>
虽然创建一个node的子类类似于创建一个UIView的子类，但是还是要遵循一些指导来确保你能充分发挥ASDK的潜力，并且你的node能够像期望的那样来工作。
### `-init`

当使用nodeBlocks时，这个方法在**后台线程**调用。因为当-init方法执行完成之前，并不会有其他的方法执行，所以在这个方法中不需要加锁。

**最重要的事情是你要记住你的init方法必须能够在任何线程中都能调用**。最为重要的是你绝对不能初始化任何UIKit对象，访问node的view和layer属性（比如：node.layer.x或node.view.x）或者添加手势。把这些东西放到`-didLoad`中
### `-didLoad`

这个方法在概念上类似于UIViewController的`-viewDidLoad`方法，他是后台view被加载的时间点。它可以确保是在**主线程**调用，并且它也是合适的地方来执行任何UIKit相关的代码。（添加手势，获取node的view，初始化UIKit组件）
### `-layoutSpecThatFits:`

这个方法在**后台线程**定义布局和执行复杂计算。这个方法是你构建layout spec对象的地方，这些layout spec会指定node的size，以及所有subsides的size和position。这里可以放置你的大部分layout代码

你创建的layout spec 对象可以随意编辑，直到它通过这个方法返回。在这之后，它会变为不可变。重要的是要记住，不要为了后续的使用而缓存layout，同样不要当必要的时候又重新创建它。

因为这个方法在后台线程运行，所以你不应该访问任何node.view或node.layer属性。同样，除非你知道你在干什么，否则不要在这个方法中创建任何node。此外，**与其他复写的方法不同，这个方法不需要执行super**
### `-layout`  

调用super之后，上面方法中返回的layoutSpec会被应用到具体的布局中。在这个方法中调用super之后，layout spec将会被计算，所有的subsides已经被测量和定位。也就是说此时你可以获取到每个subnode的位置。

`-layout`在概念上类似于UIViewController的`-viewWillLayoutSubviews`。这是一个很好的地方来改变hidden属性，如有必要可以设置一些view相关的属性（不是layout able属性）或者设置backgroundColor。你也可以把background color的设置放到layoutSpecThatFits:方法中，但这可能会有时序上的问题。**如果你有可能使用UIViews，你可以在这里设置他们的frame**。然而，你总是可以使用`-initWithViewBlock:`创建一个node封装，然后在后台线程设置他们的尺寸。

**这个方法在主线程执行**。然而，如果你在使用layout specs，你或许不应该过多依赖这个方法，因为layout specs更适合在主线程之外来布局。小于10分之1的subclasses才需要这个。

`-layout`一个很好的用法就是你希望一个subnode拥有一个具体的size。比如：当你希望collectionNode来铺满屏幕。layout specs并不支持这种情况，但在这个方法中只需要一行代码就可以轻松搞定。
\`\`\`
subnode.frame = self.bounds;
\`\`\`

如果你希望在ASViewController中有相同的效果，你可以在`-viewWillLayoutSubviews`中做同样的事情，除非你的node是在`initWithNode:`中使用的node，只有这种情况下才能达到效果。

## ASViewController
<br>
`ASViewController`是`UIViewController`的子类，它拥有特别的特性来管理nodes。**因为他是UIViewController子类，所以所有的方法都是在主线程调用的**（**你应该总是在主线程上创建一个ASViewController**）。
### `-init`

在ASViewController的生命周期中，这个方法只会执行一次。随着UIViewController的初始化，最佳的实践就是**绝对不要**在这个方法中访问`self.view`或者`self.node.view`，这会强制过早的创建view。不过，你可以在`-viewDidLoad`中访问view。

ASViewController指定的初始化方法是`-initWithNode:`。一个典型的初始化方法看起来应该跟下面的一样。需要注意的是，**在调用super之前**，ASViewController的node就要被创建。ASViewController管理node类似于UIViewController管理view的方式，只是初始化方法略微不同。
\`\`\` 
- (instancetype)init
{
  \_pagerNode = [[ASPagerNode alloc] init];
  self = [super initWithNode:\_pagerNode];
  
  // setup any instance variables or properties here
  if (self) {
	_pagerNode.dataSource = self;
	_pagerNode.delegate = self;
  }
  
  return self;
}
\`\`\`
### `-loadView`

我们建议你不要使用这个方法，因为它跟`-viewDidLoad`相比没什么特别的好处，反而有一些坏处。然而，只要你不要把`self.view`设置成不同的值，还是可以安全的使用它的。调用[super loadView]会将`self.view`设置为`node.view`。
### `-viewDidLoad`  

ASViewController的生命周期中，这个方法只会调用一次，紧随`-loadView`其后。这是最早的时间点来访问node的view。这是个很好的地方来放置只运行一次的代码和必须访问view的代码，比如添加手势。
Layout code should never be put in this method, because it will not be called again when geometry changes. Note this is equally true for UIViewController; it is bad practice to put layout code in this method even if you don't currently expect geometry changes. 
这个方法中永远都不应该放置layout相关的代码。因为当几何变化的时候，这里的代码不会再次执行。这对UIViewController而言也是一样。把layout代码放到这个方法中不是一个好的想法，即使你现在不关心几何变化。
### `-viewWillLayoutSubviews`  

这个方法跟node的`-layout`方法同时调用，并且它在ASViewController的生命周期中可能会执行很多次。当ASViewController的node的bounds改变（包括旋转，键盘显示等）和视图层级发生改变（视图的添加、删除和尺寸变更）时，这个方法都会再次执行。
  
为了统一，最好将所有的布局相关的代码放到这个方法中。因为它不会非常频繁的执行，甚至并不直接依赖size的代码也应该放在这里。
### `-viewWillAppear:` / `-viewDidDisappear:`

这些方法只有当ASViewController的node出现在屏幕上之前或之后。这些方法提供了很好的机会来执行或停止controller出现或消失相关的动画代码。这也是个记录用户行为的好地方。
Although these methods may be called multiple times and geometry information is available, they are not called for all geometry changes and so should not be used for core layout code (beyond setup required for specific animations). 
尽管这些方法会调用多次，几何信息也是可用的，但是他们并不会在所有的几何变化时都会执行。所以他们不能用来做核心layout。