---
title: 智能预加载 
layout: docs
permalink: /docs/intelligent-preloading.html
prevPage: installation.html
nextPage: containers-overview.html
---

尽管node能够被异步渲染和度量使得ASDK相当给力，但是对它而言另一个至关重要的点就是**智能预加载**
  
就像在<a href = "getting-started.html">入门</a>中指出的一样，在node container之外使用node不是那么有利。这是因为所有的nodes都维护一个自己的状态。

**所有的containers都会在内部创建和维护一个`ASRangeController`来不断的更新`interfaceState`这个属性**。

在container之外使用的node不会被任何range controller 更新状态。当node被渲染的时候这有时会导致闪屏。
## Interface State Ranges

当nodes被添加到一个scrolling或paging界面时，他们会在下面的ranges中。这意味着，当滚动了scrolling view，他们的interface states就会随着他们的移动而更新

node的状态会在下列状态中变化：
<ul>
    <li><strong>Fetch Data Range:</strong> The furthest range out from being visible. This is where content is gathered from an external source, whether that’s some API or a local disk.</li>
    <li><strong>Display Range:</strong> Here, display tasks such as text rasterization and image decoding take place.</li>
    <li><strong>Visible Range:</strong> The node is onscreen by at least one pixel.</li>
</ul>

## ASRangeTuningParameters

The size of each of these ranges is measured in "screenfuls".  While the default sizes will work well for many use cases, they can be tweaked quite easily by setting the tuning parameters for range type on your scrolling node.
这些ranges的size都使用"screenfuls”来度量。尽管大多数使用场景下默认的size已经效果很好，但是可以通过在你的scrolling node上设置tuning parameters for range type来相当轻松的调整他们。

<img src="/static/images/intelligent-preloading.png">

在上面的Pinterest的首页中，用户正在向下滚动列表。你可以看到，leading direction的ranges的size比用户正远离的内容（trailing direction）的尺寸要大很多。如果用户改变了滚动方向，为了保证内存优化，leading和trailing也会动态交换。这允许你只需要关心leading和trailing的size，而不用关心用户滚动的方向。

在这个例子中，你也可以看到在多个视图层次中智能预加载是如何工作的。在Pinterest的首页中，你可以看到一个水平的包含图片的滚动元素。这些元素在屏幕上并不可见（red），但是它拥有自己range controller来管理node的Fetch Data（yellow）和Display（orange）

## Interface State Callbacks

随着用户滚动，nodes在这些ranges中不断变化，通过loading data，rendering等行为做出合适的响应。通过实现相应的回调方法，你自定义的<a href = "subclassing.html">node subclasses</a>可以轻松的实现这种机制。

### Visible Range
`- (void)visibilityDidChange:(BOOL)isVisible;`

### Display Range
`- (void)displayWillStart`<br/>
`- (void)displayDidFinish`<br/>

### Fetch Data Range
`- (void)fetchData`<br/>
`- (void)clearFetchedData`<br/>

一定要记得调用super，OK? 😉