---
title: Node 子类
layout: docs
permalink: /docs/node-overview.html
prevPage: containers-overview.html
nextPage: subclassing.html
---
  
ASDK提供了一下node。  

<table style="width:100%" class = "paddingBetweenCols">
  <tr>
    <th>ASDK Node</th>
    <th>UIKit 参照</th> 
  </tr>
  <tr>
    <td><a href = "display-node.html"><code>ASDisplayNode</code></a></td>
    <td>代替 UIKit's <code>UIView</code><br> 
        <i>ASDK中所有node的基类</i></td> 
  </tr>
  <tr>
    <td><a href = "cell-node.html"><code>ASCellNode</code></a></td>
    <td>代替 UIKit's <code>UITableViewCell</code> & <code>UICollectionViewCell</code><br>
        <i><code>ASCellNode</code>用于 <code>ASTableNode</code>, <code>ASCollectionNode</code> and <code>ASPagerNode</code>.</i></td> 
  </tr>
  <tr>
    <td><a href = "scroll-node.html"><code>ASScrollNode</code></a></td>
    <td>代替 UIKit's <code>UIScrollView</code>
        <p><i>这个node用于创建一个自定义的可滚动的包含其他node的区域</i></p></td> 
  </tr>
  <tr>
    <td><a href = "editable-text-node.html"><code>ASEditableTextNode</code></a><br>
        <a href = "text-node.html"><code>ASTextNode</code></a></td>
    <td>代替 UIKit's <code>UITextView</code><br>
        代替 UIKit's <code>UILabel</code></td> 
  </tr>
  <tr>
    <td><a href = "image-node.html"><code>ASImageNode</code></a><br>
        <a href = "network-image-node.html"><code>ASNetworkImageNode</code></a><br>
        <a href = "multiplex-image-node.html"><code>ASMultiplexImageNode</code></a></td>
    <td>代替 UIKit's <code>UIImage</code></td> 
  </tr>
  <tr>
    <td><a href = "video-node.html"><code>ASVideoNode</code></a><br>
        <code>ASVideoPlayerNode</code></a></td>
    <td>代替 UIKit's <code>AVPlayerLayer</code><br>
        代替 UIKit's <code>UIMoviePlayer</code></td> 
  </tr>
  <tr>
    <td><a href = "control-node.html"><code>ASControlNode</code></a></td>
    <td>代替 UIKit's <code>UIControl</code></td>
  </tr>
  <tr>
    <td><a href = "button-node.html"><code>ASButtonNode</code></a></td>
    <td>代替 UIKit's <code>UIButton</code></td>
  </tr>
  <tr>
    <td><a href = "map-node.html"><code>ASMapNode</code></a></td>
    <td>代替 UIKit's <code>MKMapView</code></td>
  </tr>
</table>

<br>
Despite having rough equivalencies to UIKit components, in general, AsyncDisplayKit nodes offer more advanced features and conveniences. For example, an ASNetworkImageNode does automatic loading and cache management, and even supports progressive jpeg and animated gifs. 
尽管大体上与UIKit的组件类似，但是一般来说，AsyncDisplayKit的node提供更先进的功能和便利。比如，ASNetworkImageNode可以自动加载和缓存，甚至支持jpeg进度和动态的gif图片。
The <a href = "https://github.com/facebook/AsyncDisplayKit/tree/master/examples/AsyncDisplayKitOverview">`AsyncDisplayKitOverview`</a> 
 示例程序展示了上述所有node的用法

# Node 继承层次

所有的node都继承自 `ASDisplayNode`. 

<img src="/static/images/node-hierarchy.png" alt="node inheritance flowchart">

The nodes highlighted in blue are synchronous wrappers of UIKit elements.  For example, ASScrollNode wraps a UIScrollView, and ASCollectionNode wraps a UICollectionView.  An ASMapNode in liveMapMode is a synchronous wrapper of UIMapView.
蓝色高亮显示的nodes同步封装了UIKit元素。例如：ASScrollNode封装了UIScrollView，ASCollectionNode封装了UICollectionView，**liveMapMode中的ASMapNode同步封装了一个UIMapView**。

 
 