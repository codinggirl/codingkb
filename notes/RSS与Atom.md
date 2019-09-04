---
tags: [GitHub]
title: RSS与Atom
created: '2019-09-04T08:05:43.566Z'
modified: '2019-09-04T08:06:20.107Z'
---

# RSS与Atom

- [Atom (标准) - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-cn/Atom_(%E6%A8%99%E6%BA%96))

## RSS： 简单协议使得互联网可编程

---

*2001年有关于[肯德基的炸薯条断顿](http://magazine.cfiin.com/2001-9/3.asp)的事件报道。从中可以看到一种更高效的管理体系：对于快餐店这样全球性企业来说：要保证各地提供的薯条品质基本一致，成本最低的方法肯定是依靠机器而不是厨师，如果薯条机处理的土豆形状不一，机器的复杂程度和维护成本都会很高。所以土豆必须严格符合工业标准才能让结构比较简单的薯条机生产出符合标准的薯条。RSS和肯德基的土豆标准是一样的，体现了社会分工的细化：简单/可靠的规格意味更低高效的分工和更丰富的应用。*

[什么是RSS](http://zh.wikipedia.org/wiki/RSS): Real Simple Syndication最能体现RSS的本意
对于应用服务的开发者来说：应用和应用之间，企业和企业之间交换的数据好比就是土豆，白菜，按照严格的XML标准设计的接口的确能大大简化下游开发的后期加工机器成本：可以比较一下处理HTML网页的浏览器，比如：IE和FireFox等软件安装后大小都在10M以上，但一般处理XML的解析器工具包一般都在几百K就够了。这点在未来2，3年，随着移动终端的发展，像手机这样的硬件配置比较低的设备环境中显得尤其重要。

套用生产/代理/零售模式：而将这各个环节高效联系起来的：正是RSS/XML相关标准。
生产商：RSS生产者包括Blog / 新闻网站等；
代理商：RSS聚合服务： FeedBurner/ RSS搜索服务 TechnoRati
零售商：RSS阅读器(RSS Reader/Browser)
从中也可以看到一些MVC(The Model\-View\-Controller)模式的影子。

下面一些例子：看看RSS如何让互联网变得更加丰富

RSS的可编程性：以在线书签服务del.icio.us为例

从自身界面上看，del.icio.us是非常简朴的：
![rss_delicious.png](http://www.chedong.com/blog/archives/rss_delicious.png)

但del.icio.us为其他应用准备了各种RSS接口：

最简单的RSS应用：del.icio.us提供的RSS首先可以被其他网站进行远程同步：比如我将我的书签当成一个LinkBlog: 和我自己的常看的几个BLOG聚合一起，同步在我的[个人门户](http://www.chedong.com)上：一页天下晓。
![rss_lilina.png](http://www.chedong.com/blog/archives/rss_lilina.png)

更有其他人利用del.icio.us开放的RSS接口发展了更丰富的应用，比如：[extispicious](http://kevan.org/extispicious.cgi?name=chedong)则可以根据你收藏的书签的分类tag的个数展现你的“脑图”；
![rss_extispicious.png](http://www.chedong.com/blog/archives/rss_extispicious.png)

[HubLog: Graph del.icio.us related tags](http://hublog.hubmed.org/archives/001049.html)更是将del.icio.us所有用户收藏用的tag进行了汇总分类，可以看到不同用户收藏的Tag之间的“立体”联系：
![rss_deltags.png](http://www.chedong.com/blog/archives/rss_deltags.png)

从这些应用中可以看到：如果基于传统的HTML，同样的功能实现将变得非常复杂和不稳定，数据的再生产和交换成本是很高的。所以：**RSS这个标准最终要的贡献就是使得互联网的大部分网站变得可编程**：类似的例子还有Blog中的：TrackBack Ping等机制，这些机制都是依赖XML/RPC实现的。当初为[Lucene设计一个RSS/XML的接口](http://www.chedong.com/tech/weblucene.html)也是为了这个初衷，它使得全文检索服务可以轻松的嵌入到各种应用中，通过关键词将各种内容之间实现更丰富的关联(Well Referenced)。

其他的一些RSS扩展服务介绍：

RSS阅读：在线服务 Vs. 客户端
客户端的确是可以方便一些将RSS做为日常工作高端用户：
![rss_sharpreader.png](http://www.chedong.com/blog/archives/rss_sharpreader.png)
很多工具（RadioUserLand NewzCrawler）可以设置BLOG发布系统的帐号：MT Blogger在阅读过程中边看边发布评论，非常适合网络“蜜蜂”使用。

但是和EMail一样：习惯基于WEB界面的Email还是要占大多数，所以在线服务还是会胜出的。而RSS阅读功能最后会被大多数EMail客户端所集成，比如目前的ThunderBird。
![rss_thunderbird.png](http://www.chedong.com/blog/archives/rss_thunderbird.png)

RSS作为XML聚合再发布工具：RSS代理商
如果你同时用del.icio.us的Link Blog，还喜欢用FlickR的photo blog，同时写自己的MovableType/WordPress网志和Blogger.com的服务，如何方便用户通过一个RSS订阅你所有的信息源呢？FeedBurner的功能原不止：它可以在RSS中组合多种数据源。
原先原先需要进行多次订阅的数据源：现在用 [http://feeds.feedburner.com/blog2](http://feeds.feedburner.com/blog2)这一个URL即可，并且还添加了XSLT，可以比较高版本的IE浏览器中直接看到可读性较好的内容：
![rss_feedburner_admin.png](http://www.chedong.com/blog/archives/rss_feedburner_admin.png)
Feedburner还提供一些商业性内容的嵌入，比如：Amazon的广告推广和最近推出的Google AdSense支持等。

很多RSS上面都有Add My Yahoo! / Subscribe with BlogLines这样的连接：
![rss_subscripb.png](http://www.chedong.com/blog/archives/rss_subscripb.png)
都是由原来的分布式的用户间RSS订阅变成了一些半中心化的服务：中心化的服务的好处在于代理分工，可以节省BLOG发布者的RSS带宽，由于很多RSS软件每隔一定时间就来服务器上，这样对于一些受到带宽限制的虚拟主机用户来说，经常就会出现带宽不够的情况，转向到中心的大的代理服务商，的确是一个解决办法。而中心的 **代理**商由于为很多RSS订户服务，因此可以充分利用本地的缓存提高派送效率：同时也减少了对RSS数据源的访问压力。因此：对于RSS中心服务商来说：主要要解决就是一个[缓存机制](http://www.chedong.com/tech/cache.html)问题：
USER1 \\ / RSS1
USER2 \-\[Rss Cache\] \- RSS2
USER3 /
虽然有一些 [中心化的风险](http://www.chedong.com/blog/archives/000738.html)：但是就像大家已经很少自己做馒头一样，更多丰富的功能添加还是应该由专业代理服务商进行的。

其他一些特色服务，RSS的死链检查和容错性设计：
如果RSS数据源出现错误，BlogLines会进行一些提示。RSS浏览器如果真的非常依赖严格的XML解析器，我想最终胜出的仍然会是RSS容错性最好的工具和提供校验服务的平台。现在不规范的RSS真是太多了，RSS的URI设计也需要注意尽量避免使用动态网页：这样在OPML导出中可以避免很多&没有转移导致的XML解析错误。

基于点击统计的用户行为分析：
Attention.XML这个标准专门基于用户点击的统计输出：类似与Windows桌面上的动态开始菜单。可以根据用户的点击行为（时间，URL，内容统计）反馈给内容的发布者。

RSS之间的SNS分析：
RSS的出现也降低了搜索引擎抓取的难度： [TechnoRati](http://www.technorati.com/)是一个BLOG搜索引擎（后台也是[Lucene](http://lucene.apache.org/java/docs/index.html)），数据源是通过用户提交BLOG实现的：还会提示在TechnoRati会员之间的相互引用关系
![rss_technorati.png](http://www.chedong.com/blog/archives/rss_technorati.png)

RSS的自动分类：
RSS是草根新闻，为了提高很多用户约定俗成都用一些组合词表达一些特殊的主题，形式很像Wiki用的CamelWord：10PlacesInMyCity 10BooksIRead Yahoo360Share。非常的SearchFriendly 可以将RSS背后的个人和他们之间的引用关系可以交织出一个更复杂的人际关系网络视图，这之间的链接关系和信用机制和搜索引擎对网页Ranking机制是非常近似的。

RSS做为在线书签：
如果你收藏了上百个RSS后，RSS的书签管理就是很大的问题了，OPML就是相应的XML标准格式，一般RSS阅读工具都支持OPML的RSS书签导入和导出。我自己的OPML： [http://www.chedong.com/cache/opml.xml](http://www.chedong.com/cache/opml.xml)。

小结：
我觉得RSS是目前在线新闻媒体的进化：内容的生产者(blogger)和内容的分发者(feedburner/bloglines)和内容的接收者(rss reader/浏览器)都因为RSS这个标准而有了更多的选择和展现方式。和以往的新闻模式不同，基于RSS的新闻传播：数据源更广泛（BLOGGER），用户的可选择余地更多（代理商），用户的可定制化程度更高(浏览器) ，而不一定都要去忍受目前很多新闻网站过大的首页：[Amazon的Lite版](http://www.kokogiak.com/amazon/)就是很好的例子。

内容的提供商会更加专心于内容的开发，有了简单的API后，很多网站可以将原来界面比较复杂的网站都可以做得更LITE点，成为更好的分销商。不过RSS也使得网站内容的复制/镜像变得非常容易，这对于搜索引擎来说可不是什么好消息。

参考阅读：
[对Picks、RSS和Arggregator的若干考察](http://woooh.com/post/99.html)；

有相关的土豆标准 [NAPPO Potato Standard](http://www.yisou.com/search?p=NAPPO+Potato+Standard&lang=all&u=www.nappo.org)：好像也是米国用来设置贸易壁垒的工具；

关于标准的重要性，TinyFool说的很好：[标准，要挣钱除了标准还是标准](http://blog.tinydust.net/diary/post/26.html "标准，要挣钱除了标准还是标准")，但很多BSP显然还是对标准缺乏重视：[解析过程中经常出错](http://www.chedong.com/blog/archives/000686.html)。仅仅表面上提供RSS/XML的URL还不够酷：让人家能在各种终端方便的进行数据同步并利用你的接口开发出更丰富多彩的应用才是最重要的。

2005\-05\-07
RSS做为一个相对通用的标准：体现了 [CMS](http://www.chedong.com/tech/cms.html)的几个最重要的因素
时间：
作者：
标题：
内容摘要：

做为一种轻量级的数据交换协议：其实互联网上几乎大部分的内容都是可以用这几个因素进行相互的交换：你的邮件、工作笔记、外来的资料：blog、网摘、天气预报……所有信息全部可以用RSS进行统一的管理，更多的功能可以参见 [利用RSS可以做的15件事](http://timyang.com/comments.php?id=630_0_1_0_C)。发挥想象力，看谁能利用RSS开发出更优秀的应用。

2005\-11\-25
FeedBurner的这幅图：
[![](http://www.burningdoor.com/feedburner/archives/images/venns.gif)](http://www.burningdoor.com/feedburner/archives/001518.html)

作者：[车东](http://www.chedong.com) 发表于：2005\-04\-29 18:04 最后更新于：2007\-04\-15 19:04
[版权声明](http://creativecommons.org/licenses/by/3.0/deed.zh)：可以转载，转载时请务必以超链接形式标明文章 [RSS： 简单协议使得互联网可编程](http://www.chedong.com/blog/archives/000772.html) 的原始出处和作者信息及[本版权声明](http://www.chedong.com/blog/archives/001249.html)。
[http://www.chedong.com/blog/archives/000772.html](http://www.chedong.com/blog/archives/000772.html)

