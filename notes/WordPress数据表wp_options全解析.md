---
title: WordPress数据表wp_options全解析
created: '2019-09-04T10:17:35.143Z'
modified: '2019-09-04T10:17:37.225Z'
---

# WordPress数据表wp\_options全解析

[![wordpress主机，博客主机](http://wopus.org/images/wopusidc728_1.gif "wordpress主机，博客主机")](http://idc.wopus.org)

WordPress安装之后，会生成10个数据表，其中有一个数据表名称是：wp\_options。Wopus今天就带大家认识一下这个数据表。

**wp\_options一共包含5个字段和2个字段索引。**

**WordPress官方文档对这个数据表的介绍：**

![wp_options](http://help.wopus.org/wp-content/uploads/idc/2009/12/wp_options.jpg "wp_options")

这个数据表主要存储的数据是和WordPress后台的设置对应，平常在WordPress控制面板–设置 里进行的设置，都会对应的存储在这里。

这个数据表是我们最长用到的，但由于很多朋友对数据库不是很懂，不敢操作数据库，今天[Wopus](http://www.wopus.org)就针对这个数据表做两个典型的问题介绍，各位今后遇到相同的问题，可以按照Wopus的方法来。

**一，给自己的博客地址加上www。**

> 很多[WopusIDC](http://idc.wopus.org/announce/511.html)用户在安装WordPress之后，总遇到的一个问题是，即使自己在[域名](http://idc.wopus.org/announce/483.html)管理的地方把www.xxx.com和xxx.com都解析到了服务器的IP地址，但是访问博客，不管是加不加www，博客打开之后，地址栏里的地址总没有www。
>
> 原因很简单，各位在登陆之后，只要到设置–基本设置这里看看，把博客地址和WordPress 安装地址 安装地址都加上www，保存之后，以后不管访问xxx.com还是www.xxx.com，都会转到www.xxx.com。

**二，修正WordPress链接地址**

> 刚才有一位WopusIDC用户说，他不小心在后台把WordPress 安装地址 和博客地址都修改了，保存之后出现了严重的问题，不但修改之后的域名没有生效，就连访问原来的地址，博客样式表也全都没了。
>
> 很多WordPress新手想给博客更换主域名的时候，会这么操作，直接在WordPress后台操作，这样做的结果就是上面描述的。

**正确的操作方法：**

通过主机控制面板登陆数据库，然后找到当前的数据库，点开当前的数据库，找到wp\_options数据表。

![wp_options_1](http://help.wopus.org/wp-content/uploads/idc/2009/12/wp_options_1.jpg "wp_options_1")

点击 数据表 前面的小框，鼠标悬停是：浏览。

这样我们可以详细在浏览器右侧主体部分看到这个数据表详细的字段值。

WordPress控制面板后台的 WordPress安装地址和博客地址分别对应：（博客地址对应home，WordPress安装地址对应siteurl。）

[![siteurl](http://help.wopus.org/wp-content/uploads/idc/2009/12/siteurl.jpg "siteurl")](http://help.wopus.org/wp-content/uploads/idc/2009/12/siteurl.jpg)

[![home](http://help.wopus.org/wp-content/uploads/idc/2009/12/home.jpg "home")](http://help.wopus.org/wp-content/uploads/idc/2009/12/home.jpg)

我们只需要按照正确的路径，修改这两个地址，这样就可以完成博客域名的替换，遇到的问题也就解决了。

**三，博客转移主机之后，无法上传附件的解决办法**

> 很多朋友，因为对[WordPress主机](http://idc.wopus.org)认识有一个不断变化的过程，所以会经常更换主机，几次转换之后，还有可能出现的一个问题是：博客无法上传附件。
>
> 出现问题的原因：不管是哪种主机，在帐户创建的时候，那Wopus来说，帐户的绝对路径开始一定是：/home/wopus/xxxx。这里wopus就是登陆主机控制面板的用户名，如果把博客放在public\_html根目录，那附件上传的绝对路径是：/home/wopus/public\_html/wp\-content/uploads/month/xxx。
>
> 由于要换空间，新开空间可能会使用不同的用户名，比如新的空间使用qiuzhang这个用户名，那么，在程序上传完毕，数据库也导入完成之后，附件的上传绝对路径还是没有变，还是：/home/wopus/public\_html/wp\-content/uploads/month/xxx；但正确的上传绝对路径应该是：/home/qiuzhang/public\_html/wp\-content/uploads/month/xxx。
>
> 于是，我们只要在wp\_options数据表的第二页找到：upload\_path，点击最前面的修改，把地址修改成正确的绝对地址，然后保存，这样，再上传，问题就解决了。

关于WordPress数据表wp\_options，就讲这三个问题，这三个问题也是新手朋友经常遇到的，希望能帮助各位能解决问题。

