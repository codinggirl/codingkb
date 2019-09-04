---
title: 渐进式 JPEG 图片和交错式 PNG/GIF 图片
created: '2019-09-04T10:36:51.482Z'
modified: '2019-09-04T10:41:21.782Z'
---

# 渐进式 JPEG 图片和交错式 PNG/GIF 图片

[https://www.thjiang.com/2017/04/11/渐进式JPEG图片和交错式PNG\-GIF图片/](https://www.thjiang.com/2017/04/11/%E6%B8%90%E8%BF%9B%E5%BC%8FJPEG%E5%9B%BE%E7%89%87%E5%92%8C%E4%BA%A4%E9%94%99%E5%BC%8FPNG-GIF%E5%9B%BE%E7%89%87/)

2017\-04\-11

*   [性能优化](https://www.thjiang.com/categories/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/)

545

最近发现知乎的图片改成了渐进式的，和常见的懒加载一样使用小图片来占位，然后替换成渐进式大图。体验链接：[https://www.zhihu.com/question/19647535](https://www.zhihu.com/question/19647535)

在网络中，我们常见的图片是从上到下一条一条逐渐加载的，这种JPEG叫做Baseline JPEG，压缩模式为顺序式编码（Sequential Encoding），当图片较大时要等待图片逐行加载很久我们才能对图片有一定了解。
在上面的链接中，知乎的图片在加载瞬间就已经可以让我们看到大致轮廓，这就是渐进式JPEG（Progressive JPEG），它使用了递增式编码（Progressive Encoding）。

交错式PNG和GIF图片也有类似效果，它是隔行扫描加载的，可以让用户只花50%的时间就能看到图片全貌。

在HTTP2中，有2个很重要的特点，多路复用（允许同时通过单一的 HTTP/2 连接发起多重的请求\-响应消息）和服务器推送（服务器对同一请求可以推送多个响应），它们对渐进式图片的加载速度有极大帮助。

在多路复用的连接中，可以将资源区分优先级，使他们更快的加载。JPEG图片默认包含10个图层，最终的图片是由10个图层叠加而成的。渐进式JPEG的第一个视觉图层通常是高像素黑白的，因为它节省了颜色通道信息。在支持启用HTTP2的Web服务中，可以将渐进式JPEG的图像扫描层标记为高优先级，甚至可以实现在相应的图片请求开始之前就将这些扫描层推送到客户端浏览器的push缓存中。

### [](#如何实现 "如何实现")如何实现

在Photoshop中，按住 `Ctrl+Alt+Shift+S` 存储为WEB和设备所用格式

渐进式图片：选择图片格式为JPEG => 选中“连续”
交错式图片：选择图片格式为PNG/GIF => 选中“交错”

### [](#参考链接 "参考链接")参考链接

[http://www.zhangxinxu.com/wordpress/2013/01/progressive\-jpeg\-image\-and\-so\-on/](http://www.zhangxinxu.com/wordpress/2013/01/progressive-jpeg-image-and-so-on/)
[https://calendar.perfplanet.com/2016/even\-faster\-images\-using\-http2\-and\-progressive\-jpegs/](https://calendar.perfplanet.com/2016/even-faster-images-using-http2-and-progressive-jpegs/)
[http://adavu.com/what\-is\-baseline\-jpeg\-and\-progressive\-jpeg/](http://adavu.com/what-is-baseline-jpeg-and-progressive-jpeg/)
[https://en.wikipedia.org/wiki/JPEG](https://en.wikipedia.org/wiki/JPEG)

> 本文地址：[https://www.thjiang.com/2017/04/11/渐进式JPEG图片和交错式PNG\-GIF图片/](https://www.thjiang.com/2017/04/11/%E6%B8%90%E8%BF%9B%E5%BC%8FJPEG%E5%9B%BE%E7%89%87%E5%92%8C%E4%BA%A4%E9%94%99%E5%BC%8FPNG-GIF%E5%9B%BE%E7%89%87/)

