---
title: 入门
layout: docs
permalink: /docs/getting-started.html
nextPage: resources.html
---

`node`是AsyncDisplayKit的基本单元。`ASDisplayNode `是UIView上的抽象，正如UIView是CALayer上的抽象。与views只能用于主线程不同，**nodes是线程安全的。你可以在后台线程同时初始化并且配置他们的全部层级**。
为了保证APP的用户界面的平滑和反应灵敏，它应该每秒渲染60帧，这是iOS的黄金标准。这意味着主线程有60分之1秒的时间来push每一帧。也就是说只有16毫秒的时间来执行所有的布局和绘制代码！由于系统开销，在APP引起丢帧之前，你的代码通常只有小于10毫秒的时间来运行。

ASDK让你将图片编码、文本尺寸计算和渲染以及其他的昂贵的UI操作放到主线程之外进行，这样保证主线程有空闲时间来响应用户交互。AsyncDisplayKit还有其他的技巧，我们随后可以知道。

<h2><a href = "node-overview.html">Nodes</a></h2>

如果你经常和views打交道，那么你已经知道如何使用nodes了。views的大多数方法都和node相同，并且大多数UIView和CALayer的属性也都是可用的。在任何命名冲突的情况下（如 .clipsToBounds 和 .masksToBounds），**nodes默认使用UIView的命名。唯一的例外就是nodes使用position来代替center**。

当然，你可以直接通过<code>node.view</code> or <code>node.layer</code>的方式来访问底层的view或layer，**只要保证在主线程这么做**。

AsyncDisplayKit提供了很多\<a href = "node-overview.html"\>nodes\</a\>来替换你之前使用的大部分UIKit组件。大型app已经能够仅使用AsyncDisplayKit的nodes来完全书写他们的UI。

<h2><a href = "containers-overview.html">Node 容器</a></h2>
  
当转换一个app来使用AsyncDisplayKit时，一个**常见的错误**就是直接把nodes添加到一个已经存在的view层级中。这样操作会导致nodes在渲染的时候出现闪屏。

所以，你应该将nodes作为<a href = "containers-overview.html">node container classes</a>的subnodes，添加到上面去。这些containers负责告诉submodes他们当前处于什么状态，这样就能尽可能有效率的来加载数据和渲染node。**你应该认为把这些node container classes当做是UIKit和ASDK之间的集成点**（integration point）。

<h2><a href = "/docs/layout-engine.html">Layout 引擎</a></h2>

AsyncDisplayKit的layout引擎是它最强力和最独特的特性之一。**基于CSS的FlexBox模型**，它提供了一个**声明式**的方式来指定一个自定义node的size和在subnodes中的layout。虽然通过为每个node提供一个ASLayoutSpec，nodes可以异步度量和布局，但是所有的nodes默认同时渲染。

<h2><a href = "/docs/layout-engine.html">高级开发特性</a></h2>

ASDK提供了很多UIKit或Foundation无法找到的高级开发特性。我们的开发者已经发现ASDK在他们的架构中允许简化和提高开发速度。

<h2><a href = "/docs/layout-engine.html">添加ASDK到你的APP</a></h2>

如果你是刚接触ASDK，我们推荐你先熟悉下我们的`ASDKgram`示例程序。
如果你在这个过程中遇到了问题，可以来<a href = "/docs/resources.html#slack"> 这里 </a>  这里寻求帮助。