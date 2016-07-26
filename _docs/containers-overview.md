---
title: Node 容器
layout: docs
permalink: /docs/containers-overview.html
prevPage: intelligent-preloading.html
nextPage: node-overview.html
---

### Use Nodes in Node Containers
It is highly recommended that you use AsyncDisplayKit's nodes within a node container. AsyncDisplayKit offers the following node containers.
强烈推荐你在一个node容器中使用ASDK的node。ASDK提供了下列node容器。
<table style="width:100%" class = "paddingBetweenCols">
  <tr>
    <th>ASDK Node Container</th>
    <th>UIKit Equivalent</th> 
  </tr>
  <tr>
    <td><a href = "containers-asviewcontroller.html"><code>ASViewController</code></a></td>
    <td>in place of UIKit's <code>UIViewController</code></td>
  </tr>
  <tr>
    <td><a href = "containers-ascollectionnode.html"><code>ASCollectionNode</code></a></td>
    <td>in place of UIKit's <code>UICollectionView</code></td>
  </tr>
  <tr>
    <td><a href = "containers-aspagernode.html"><code>ASPagerNode</code></a></td>
    <td>in place of UIKit's <code>UIPageViewController</code></td>
  </tr>
  <tr>
    <td><a href = "containers-astablenode.html"><code>ASTableNode</code></a></td>
    <td>in place of UIKit's <code>UITableView</code></td>
  </tr>
</table>

<br>
Example code and specific sample projects are highlighted in the documentation for each node container. 

<!-- For a detailed description on porting an existing UIKit app to AsyncDisplayKit, read the <a href = "porting-guide.html">porting guide</a>. -->

### 通过使用node容器我能收获什么?

node容器会自动管理它内部node的<a href = "intelligent-preloading.html">智能预加载</a>。这意味着node所有的layout，data fetching，解码和渲染都会异步进行。这也是为什么推荐在node container中使用node。

需要注意的是，虽然有可能直接使用node（不包含在ASDK的node container之中），除非你添加了额外的回调，否则一旦他们出现在屏幕上，他们就会开始绘制（就像UIKit一样）。这会导致性能下降和内容闪屏。