---
tags: [GitHub]
title: RSS、Atom、Feed 介绍与简单实现
created: '2019-09-04T08:02:20.450Z'
modified: '2019-09-04T08:02:33.252Z'
---

## RSS、Atom、Feed 介绍与简单实现[](https://github.com/wenzhixin/blog/edit/master/html/posts/2013/11/08/rss_atom_feed_php.md "编辑文章（纠正错误）")

分类：后台技术 | 标签：php、RSS、Atom、Feed | 发布时间：2013\-11\-08 22:59:00

---

最近接触了 RSS 订阅相关的，做了一些了解与开发，记录下。

#### RSS是什么

RSS（全称RDF Site Summary，网景的最初定义），RSS也是一种“类网页”描述语言（或叫文档格式）， 最初由网景公司（Netscape）定义，RSS只是有个相对统一的规范（注意只是规范）， 前途未卜（RSS 2.0的版权问题）。RSS作为网站内容分享的一种便利接口，只是从博客（BLOG）风行才开始广为流传。

关于RSS的更多介绍请参考[RSS](http://zh.wikipedia.org/zh-cn/RSS)。

#### ATOM是什么

由于RSS前途未卜，而且RSS标准发展存在诸多问题或不足，于是ATOM横空出世，可理解为RSS的替代品。 ATOM是IETF的建议标准，Atom Syndication Format是基于XML格式，Atom Publishing Protocol则是基于HTTP协议格式。

RSS与ATOM比较，请参考：[ATOM](http://zh.wikipedia.org/wiki/Atom)

#### FEED是什么

FEED只是一个中间过程，所以全世界没人能给FEED下一个准确的定义，所以大家不用关心FEED的定义，其实FEED什么都不是。 如果非得给个说法，最好还是放到英文环境下理解似乎更加合理，FEED其实就是RSS（或ATOM）和订阅用户之间的“中间商”， 起到帮忙批发传递信息的作用。所以，FEED的常见格式就是RSS和ATOM，网络上说的FEED订阅，更确切的说法应该仍然是RSS或ATOM订阅。

FEED更多介绍：[Feed](http://en.wikipedia.org/wiki/Feed)

#### 可用的工具

RSS Feed 检验网站：[feedvalidator](http://feedvalidator.org)。可以检验你的 RSS 是否符合标准，假如不符合会给出相应的提示和警告。

RSS托管服务网站：[feedburner](http://feedburner.google.com)。 网站定位：全球最大的RSS托管服务网站。07年被google以1亿美元收购，现在已迁移到Google域名之下。

[RSS、Atom、Feed 介绍与简单实现 —— 文翼的博客](https://wenzhixin.net.cn/2013/11/08/rss_atom_feed_php)


