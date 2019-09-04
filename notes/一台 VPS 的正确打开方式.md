---
tags: [GitHub]
title: 一台 VPS 的正确打开方式
created: '2019-09-04T08:53:49.078Z'
modified: '2019-09-04T08:53:52.185Z'
---

一台 VPS 的正确打开方式

![Author Avatar](https://ae01.alicdn.com/kf/HTB1DdGHXizxK1RjSspj763S.pXax.png)

**Wincer** 2月 22, 2018

*devices other* devices other

*   在其它设备中阅读本文章
![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMYAAADGCAAAAACs8KCBAAAB3ElEQVR42u3aOXLDQAwEQP3/03bqwF4PAOrgbjNiiYfYCQrX42uL44GBgYGBgYGB8TGMR3z88qIfv6/vr15NvgEDA+M0xj+hLbgnQU7ej4GBgbEOi+vzdRj9K7BOvgEDAwMjYSSfmCMxMDAwXslYp3rV92NgYGD0CsikHZbf87ZaHAMD44aMvCn/vPOXzjcwMDBuxei1+3up4XzYgIGBcSajmtL1gnUywiw/hYGBcRgjGUDmwbcHTlp1GBgYZzLyVv56hSJp0iWBfpThYmBgHMOoDiOrRexkhICBgXEmY73g1StHew24atKJgYFxGqOXnFXTvkljrjyJxcDA2JqxDsHJL5ORwGUBFwMDYyNG3kTLi8zkaq9th4GBcSYjX/PKw24eWJ9SxGJgYGzKSNLEyVpq8ubqsAEDA+NkRh4iJ0203ooGBgYGRl6g9hLE3keXly0wMDC2ZszHANWmfy9lxMDAOI3RW60orERUw2ieXGJgYBzDmKxqlf84bqtdtjOCgYGxEeOqUrOaUOaBGAMDA2MySqwOLPMw3azFMTAwMAaDhHm6iYGBgTEJuNV2f+9ZDAwMjKSInTTIeqUsBgYGRnUwkAfiasCtFswYGBjnMO57YGBgYGBgYGC89fgGdVihXijN3VAAAAAASUVORK5CYII=)

*bookmark* bookmark

*   [API](https://blog.itswincer.com/tags/API/)
*   [Nginx](https://blog.itswincer.com/tags/Nginx/)
*   [SSH](https://blog.itswincer.com/tags/SSH/)
*   [VPS](https://blog.itswincer.com/tags/VPS/)

*share* share

[*   分享到微博](http://service.weibo.com/share/share.php?appkey=&title=一台 VPS 的正确打开方式&url=https://blog.itswincer.com/posts/b3085a7/index.html&pic=https://blog.itswincer.comhttps://blog.itswincer.com/img/favicon.png&searchPic=false&style=simple)[*   分享到 Twitter](https://twitter.com/intent/tweet?text=一台 VPS 的正确打开方式&url=https://blog.itswincer.com/posts/b3085a7/index.html&via=Wincer)[*   分享到 Facebook](https://www.facebook.com/sharer/sharer.php?u=https://blog.itswincer.com/posts/b3085a7/index.html)[*   分享到 Google+](https://plus.google.com/share?url=https://blog.itswincer.com/posts/b3085a7/index.html)[*   分享到 QQ](https://connect.qq.com/widget/shareqq/index.html?site=Wincer's Blog&title=一台 VPS 的正确打开方式&summary=这里是 @Wincer，喜欢折腾新技术，却没多大耐心。个人博客会写得比较杂，包括日常吐槽、技术踩坑，偶尔会写正经一点的。欢迎订阅 RSS(•̀ᴗ•́)。&pics=https://blog.itswincer.comhttps://blog.itswincer.com/img/favicon.png&url=https://blog.itswincer.com/posts/b3085a7/index.html)

其实像 Hexo 这样的静态博客框架本不需要服务器的，GitHub Pages 就提供免费的托管服务、且不限流量，但内心那点不安分因素总是撩拨着我：比如可以自定防护规则、可以搭建私有 Git 服务、可以搭建自己的 API（这个比较重点）、还能自己搭建 SS 服务，于是乎就买了一台 VPS。

由于我的博客使用了 [Cloudflare](https://www.cloudflare.com/) 作为 CDN 服务商，而国内的电信和联通用户是默认解析到 Cloudflare 的美西结点，只有移动用户是解析到香港节点，所以为了 API 的快取速度（即：本机 \-> Cloudflare \-> VPS \-> Cloudflare \-> 本机），将服务器选在了洛杉矶，每年 20$、1T 流量、10G 固态、512M 内存，搭建一个静态博客和几个 API 足够了。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%AE%80%E5%8C%96-SSH-%E7%99%BB%E5%BD%95 "简化 SSH 登录")简化 SSH 登录

SSH 的安全验证有两种级别：

1.  基于密码：知道帐号和密码，就可以登录到远程主机，这种方式无法避免「中间人」攻击
2.  基于密钥：创建一对密钥，并把公钥放至服务器，每次通信都会检验密钥，从而可以避免「中间人」攻击

这里介绍第二种方法。

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%94%9F%E6%88%90%E5%AF%86%E9%92%A5 "生成密钥")生成密钥

如果在使用 GitHub 的时候已经生成过，那么这一步可以略过

```
1ssh-keygen                        # 默认生成长度为 2048 位的 RSA 密钥2ssh-keygen -b 4096                # 可以通过添加参数 -b 设定长度
```

随后就会生成一对密钥，默认为：id\_rsa（私钥）、id\_rsa.pub（公钥）

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E4%B8%8A%E4%BC%A0%E8%87%B3%E6%9C%8D%E5%8A%A1%E5%99%A8 "上传至服务器")上传至服务器

使用 ssh\-copy\-id 命令

```
1ssh-copy-id username@server-addr
```

需要输入远程服务器的登录密码，随后 id\_rsa.pub（公钥）会自动上传至服务器的 `~/.ssh/authorized_keys` 文件中

随后再进行 SSH 连接时，就不需要再输入密码了

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%AE%80%E5%8C%96-IP "简化 IP")简化 IP

虽不用输入密码，但仍需要输入服务器登录名和 IP 地址，所以需要将配置写入 `~/.ssh/config` 中：

```
1Host wincer                # 这里填写简化名称2  HostName ××.××.××.××    # 服务器 IP3  Port 22                # 端口号4  User root                # 远程登录用户名
```

随后再进行 SSH 连接时，输入 `ssh wincer` 就可以登录了

`scp` 命令也可以简化成以下：

```
1scp FILENAME wincer:PATH
```

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E4%BD%9C%E4%B8%BA-GitHub-Pages "作为 GitHub Pages")作为 GitHub Pages

目前我的博客仍然在[该仓库](https://github.com/WincerChan/MyBlog)的 master 分支上保留有静态文件，仅作备份。

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%B7%BB%E5%8A%A0-DNS-%E8%AE%B0%E5%BD%95 "添加 DNS 记录")添加 DNS 记录

首先为 DNS 解析添加一条 「A 记录」，记录值为 VPS 所分配的 IP

![A 记录](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

A 记录

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%9B%B4%E6%94%B9-Nginx-%E9%85%8D%E7%BD%AE "更改 Nginx 配置")更改 Nginx 配置

SSH 登录后，编辑 Nginx 的配置文件 `vim /etc/nginx/nginx.conf`：

```
1server {2  listen        80;3  server_name    blog.itswincer.com;4  index         index.html;5  root             /data/www/hexo;6}
```

可部署多个子域名，只需将 `server_name` 和 `root` 替换成相应的子域名和文件夹就可以了

可以先创建一个 `index.html` 测试一下，访问 `blog.itswincer.com` 看看是否成功

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E5%8A%A0%E5%AF%86-CI-%E9%85%8D%E7%BD%AE "加密 CI 配置")加密 CI 配置

这一步可选，你也可以手动用 `scp` 命令将每次 `hexo g` 生成的静态文件上传至服务器，只不过略微麻烦。

Travis CI 的终端并不能支持用户输入密码，而 GitHub 的 Token 又无法在自己的服务器使用，故而只能采取[简化 SSH 登录](#简化 SSH 登录)这步中类似的方法，即用私钥（即 id\_rsa）去确认登录的身份，而将私钥公开至 GitHub 又是很危险的，所以我们需要将私钥加密：

```
1gem install travis        # 需要安装 gem，自备梯子2travis login            # 输入 GitHub 的账户密码3travis encrypt-file ~/.ssh/id_rsa --add        # 加密私钥，同时解密命令会添加至 travis.yml
```

Travis CI 上的 known\_hosts 只添加了 GitHub 下的三个域名，在使用 SSH 登录时，会提示是否添加该主机，同样因为终端无法输入，所以需现将服务器的 IP 与端口号添加至 known\_hosts：

```
1addons:2  ssh_known_hosts: ××.××.××.××
```

> 这里忍不住吐槽一下 Travis CI 的加密：居然无法同时加密两个文件，而官方提供的方法是先把需要加密的文件压缩后加密，再解压。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA-API "搭建 API")搭建 API

Hexo 这类静态博客所需的内存其实是挺少的，只需在后台运行一个 Nginx 进程就可以了，只运行一个 Nginx 进程时用「搬瓦工」的管理面板查看发现一共内存才使用了 40M，才用了不到 10%，所以就想着可以将之前写的「一言」API 放到我的服务器上，毕竟 Heroku 在国内访问还是挺不稳定的。

之前是用 Python3 写的，后来发现 VPS 自带的 Python 版本居然是 2.7，深知其中坑的我就没打算再用 Python 了，于是就是用 Node.JS 写了一个，本地调试了一下，就扔到服务器上了。第二天早上起来一看，发现内存占用居然到了 110M，一查看原来都是 Node 的占用，其中每一个 API 请求，平均就会多占用 2M 的内存，而且这个请求所占用的内存并不会释放，这样下去怕是没两天服务器就要爆内存了。

后来我也想过解决办法，比如用 PM2 这个工具来限制运行的内存，超过就重启 Node 环境，也想过定时重启服务器，再转念一想，我是大爷诶，凭啥要我去迁就辣鸡 Node.JS 的内存管理，你不行那我换一个具有垃圾回收的语言不就好了，那就 Java？好像也不太行，毕竟服务器就那么点硬盘，JDK 和 JRE 不知道要占用我多少空间，再者说来毕竟我可是 「Java 黑」。

那么就归纳一下我的需求：「占用内存小、部署方便、有垃圾回收（不会爆内存）、~不要 Java~」，然后考虑到编译型程序比解释型程序占用的内存更小，所以也就没考虑 Ruby & Python，满足这些要求的好像也只有 Golang 了。

写了那么久的动态类型语言，突然要我写静态类型语言还真是有点不适应。在网上找了个例子，自己捣鼓了一个下午，就写出来了，算是一个勉强遵循「RESTful」风格的 API，开始还有日志功能，后来想想没必要，Nginx 也可以监控端口的访问日志，就删去了。

然后在 Nginx 配置端口

```
1server {2  listen        80;3  server_name    api.itswincer.com;4  location / {5    add_header Content-Type 'application/javascript';6    proxy_pass http://localhost:520;7  }8}
```

而且 Golang 的部署也是很方便，将 \*.go 拷贝就行了。跑了几天，内存占用稳定在 10M 上下。

[该项目](https://github.com/WincerChan/hitokoto)已托管至 GitHub。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA-SS "搭建 SS")搭建 SS

搬瓦工的 SS/SSR 搭建可以说是非常的方便了：

1.  先进入 KiwiVM 面板
2.  在左侧点击 `Shadowsocks Server` 按钮
3.  再点击 `install Shadowsocks Server` 按钮

大约半分钟后，会提示已经安装完成：

![安装](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

安装

再点击 `Go back` 按钮，回到以下界面，再点击 `Start`：![配置](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

配置

SS 服务配置完成了，将以上信息填入 SS 客户端即可使用。

不过由于服务器是在美西，所以无论怎么优化（BBR），延迟都会在 160ms 以上，当然这对浏览网页看视频来说也没有什么影响。

> 注意：**当你使用 VPS 翻墙时，会同时计算上行、下行流量，也就是说如果翻墙使用 1G 流量，其实等于使用了 VPS 的 2G 流量**。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BA%91%E7%AC%94%E8%AE%B0 "搭建私有云笔记")搭建私有云笔记

我最近有在思考私有云笔记的必要性，毕竟有了博客，那云笔记的作用可能就鸡肋了一点。但我还是选择了搭建。我的想法是：博客用于存放、发布一些较正式的文章，而笔记可以休闲一点（类似作文和日记的区别）。

回到正题，目前来说，体验好的云笔记要么需要会员、要么存在诸多功能限制，而我又不想多浪费钱，那么选择一个支持多设备（其实主要是解决手机设备）的同步方案并借助私有的服务器架设自然也就是最好的解决办法了。我选择了 [Nextcloud 作为解决方案](https://blog.itswincer.com/posts/bf0413ac/)，并借助他的 WebDAV 功能作为多端同步工具。

手机端笔记软件选择的是[易码](https://www.coolapk.com/apk/me.tshine.easymark)，支持 Markdown 语法和 WebDAV 同步，电脑端可以选择直接用浏览器访问 Nextcloud，可以在线 Markdown 编辑和预览，当然也可以选择用 Nextcloud 同步至本地文件夹，并用其它编辑器打开就可以了。

> **本文标题：** 一台 VPS 的正确打开方式
> **最后更新：**2018 年 04 月 02 日 \- 14:04
> **本文链接：**[https://blog.itswincer.com/posts/b3085a7/](https://blog.itswincer.com/posts/b3085a7/)
> **本文采用：署名\-非商业性使用\-禁止演绎 4.0 国际** 协议进行许可，阅读 [相关说明](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)一台 VPS 的正确打开方式

![Author Avatar](https://ae01.alicdn.com/kf/HTB1DdGHXizxK1RjSspj763S.pXax.png)

**Wincer** 2月 22, 2018

*devices other* devices other

*   在其它设备中阅读本文章
![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMYAAADGCAAAAACs8KCBAAAB3ElEQVR42u3aOXLDQAwEQP3/03bqwF4PAOrgbjNiiYfYCQrX42uL44GBgYGBgYGB8TGMR3z88qIfv6/vr15NvgEDA+M0xj+hLbgnQU7ej4GBgbEOi+vzdRj9K7BOvgEDAwMjYSSfmCMxMDAwXslYp3rV92NgYGD0CsikHZbf87ZaHAMD44aMvCn/vPOXzjcwMDBuxei1+3up4XzYgIGBcSajmtL1gnUywiw/hYGBcRgjGUDmwbcHTlp1GBgYZzLyVv56hSJp0iWBfpThYmBgHMOoDiOrRexkhICBgXEmY73g1StHew24atKJgYFxGqOXnFXTvkljrjyJxcDA2JqxDsHJL5ORwGUBFwMDYyNG3kTLi8zkaq9th4GBcSYjX/PKw24eWJ9SxGJgYGzKSNLEyVpq8ubqsAEDA+NkRh4iJ0203ooGBgYGRl6g9hLE3keXly0wMDC2ZszHANWmfy9lxMDAOI3RW60orERUw2ieXGJgYBzDmKxqlf84bqtdtjOCgYGxEeOqUrOaUOaBGAMDA2MySqwOLPMw3azFMTAwMAaDhHm6iYGBgTEJuNV2f+9ZDAwMjKSInTTIeqUsBgYGRnUwkAfiasCtFswYGBjnMO57YGBgYGBgYGC89fgGdVihXijN3VAAAAAASUVORK5CYII=)

*bookmark* bookmark

*   [API](https://blog.itswincer.com/tags/API/)
*   [Nginx](https://blog.itswincer.com/tags/Nginx/)
*   [SSH](https://blog.itswincer.com/tags/SSH/)
*   [VPS](https://blog.itswincer.com/tags/VPS/)

*share* share

[*   分享到微博](http://service.weibo.com/share/share.php?appkey=&title=一台 VPS 的正确打开方式&url=https://blog.itswincer.com/posts/b3085a7/index.html&pic=https://blog.itswincer.comhttps://blog.itswincer.com/img/favicon.png&searchPic=false&style=simple)[*   分享到 Twitter](https://twitter.com/intent/tweet?text=一台 VPS 的正确打开方式&url=https://blog.itswincer.com/posts/b3085a7/index.html&via=Wincer)[*   分享到 Facebook](https://www.facebook.com/sharer/sharer.php?u=https://blog.itswincer.com/posts/b3085a7/index.html)[*   分享到 Google+](https://plus.google.com/share?url=https://blog.itswincer.com/posts/b3085a7/index.html)[*   分享到 QQ](https://connect.qq.com/widget/shareqq/index.html?site=Wincer's Blog&title=一台 VPS 的正确打开方式&summary=这里是 @Wincer，喜欢折腾新技术，却没多大耐心。个人博客会写得比较杂，包括日常吐槽、技术踩坑，偶尔会写正经一点的。欢迎订阅 RSS(•̀ᴗ•́)。&pics=https://blog.itswincer.comhttps://blog.itswincer.com/img/favicon.png&url=https://blog.itswincer.com/posts/b3085a7/index.html)

其实像 Hexo 这样的静态博客框架本不需要服务器的，GitHub Pages 就提供免费的托管服务、且不限流量，但内心那点不安分因素总是撩拨着我：比如可以自定防护规则、可以搭建私有 Git 服务、可以搭建自己的 API（这个比较重点）、还能自己搭建 SS 服务，于是乎就买了一台 VPS。

由于我的博客使用了 [Cloudflare](https://www.cloudflare.com/) 作为 CDN 服务商，而国内的电信和联通用户是默认解析到 Cloudflare 的美西结点，只有移动用户是解析到香港节点，所以为了 API 的快取速度（即：本机 \-> Cloudflare \-> VPS \-> Cloudflare \-> 本机），将服务器选在了洛杉矶，每年 20$、1T 流量、10G 固态、512M 内存，搭建一个静态博客和几个 API 足够了。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%AE%80%E5%8C%96-SSH-%E7%99%BB%E5%BD%95 "简化 SSH 登录")简化 SSH 登录

SSH 的安全验证有两种级别：

1.  基于密码：知道帐号和密码，就可以登录到远程主机，这种方式无法避免「中间人」攻击
2.  基于密钥：创建一对密钥，并把公钥放至服务器，每次通信都会检验密钥，从而可以避免「中间人」攻击

这里介绍第二种方法。

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%94%9F%E6%88%90%E5%AF%86%E9%92%A5 "生成密钥")生成密钥

如果在使用 GitHub 的时候已经生成过，那么这一步可以略过

```
1ssh-keygen                        # 默认生成长度为 2048 位的 RSA 密钥2ssh-keygen -b 4096                # 可以通过添加参数 -b 设定长度
```

随后就会生成一对密钥，默认为：id\_rsa（私钥）、id\_rsa.pub（公钥）

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E4%B8%8A%E4%BC%A0%E8%87%B3%E6%9C%8D%E5%8A%A1%E5%99%A8 "上传至服务器")上传至服务器

使用 ssh\-copy\-id 命令

```
1ssh-copy-id username@server-addr
```

需要输入远程服务器的登录密码，随后 id\_rsa.pub（公钥）会自动上传至服务器的 `~/.ssh/authorized_keys` 文件中

随后再进行 SSH 连接时，就不需要再输入密码了

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E7%AE%80%E5%8C%96-IP "简化 IP")简化 IP

虽不用输入密码，但仍需要输入服务器登录名和 IP 地址，所以需要将配置写入 `~/.ssh/config` 中：

```
1Host wincer                # 这里填写简化名称2  HostName ××.××.××.××    # 服务器 IP3  Port 22                # 端口号4  User root                # 远程登录用户名
```

随后再进行 SSH 连接时，输入 `ssh wincer` 就可以登录了

`scp` 命令也可以简化成以下：

```
1scp FILENAME wincer:PATH
```

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E4%BD%9C%E4%B8%BA-GitHub-Pages "作为 GitHub Pages")作为 GitHub Pages

目前我的博客仍然在[该仓库](https://github.com/WincerChan/MyBlog)的 master 分支上保留有静态文件，仅作备份。

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%B7%BB%E5%8A%A0-DNS-%E8%AE%B0%E5%BD%95 "添加 DNS 记录")添加 DNS 记录

首先为 DNS 解析添加一条 「A 记录」，记录值为 VPS 所分配的 IP

![A 记录](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

A 记录

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%9B%B4%E6%94%B9-Nginx-%E9%85%8D%E7%BD%AE "更改 Nginx 配置")更改 Nginx 配置

SSH 登录后，编辑 Nginx 的配置文件 `vim /etc/nginx/nginx.conf`：

```
1server {2  listen        80;3  server_name    blog.itswincer.com;4  index         index.html;5  root             /data/www/hexo;6}
```

可部署多个子域名，只需将 `server_name` 和 `root` 替换成相应的子域名和文件夹就可以了

可以先创建一个 `index.html` 测试一下，访问 `blog.itswincer.com` 看看是否成功

### [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E5%8A%A0%E5%AF%86-CI-%E9%85%8D%E7%BD%AE "加密 CI 配置")加密 CI 配置

这一步可选，你也可以手动用 `scp` 命令将每次 `hexo g` 生成的静态文件上传至服务器，只不过略微麻烦。

Travis CI 的终端并不能支持用户输入密码，而 GitHub 的 Token 又无法在自己的服务器使用，故而只能采取[简化 SSH 登录](#简化 SSH 登录)这步中类似的方法，即用私钥（即 id\_rsa）去确认登录的身份，而将私钥公开至 GitHub 又是很危险的，所以我们需要将私钥加密：

```
1gem install travis        # 需要安装 gem，自备梯子2travis login            # 输入 GitHub 的账户密码3travis encrypt-file ~/.ssh/id_rsa --add        # 加密私钥，同时解密命令会添加至 travis.yml
```

Travis CI 上的 known\_hosts 只添加了 GitHub 下的三个域名，在使用 SSH 登录时，会提示是否添加该主机，同样因为终端无法输入，所以需现将服务器的 IP 与端口号添加至 known\_hosts：

```
1addons:2  ssh_known_hosts: ××.××.××.××
```

> 这里忍不住吐槽一下 Travis CI 的加密：居然无法同时加密两个文件，而官方提供的方法是先把需要加密的文件压缩后加密，再解压。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA-API "搭建 API")搭建 API

Hexo 这类静态博客所需的内存其实是挺少的，只需在后台运行一个 Nginx 进程就可以了，只运行一个 Nginx 进程时用「搬瓦工」的管理面板查看发现一共内存才使用了 40M，才用了不到 10%，所以就想着可以将之前写的「一言」API 放到我的服务器上，毕竟 Heroku 在国内访问还是挺不稳定的。

之前是用 Python3 写的，后来发现 VPS 自带的 Python 版本居然是 2.7，深知其中坑的我就没打算再用 Python 了，于是就是用 Node.JS 写了一个，本地调试了一下，就扔到服务器上了。第二天早上起来一看，发现内存占用居然到了 110M，一查看原来都是 Node 的占用，其中每一个 API 请求，平均就会多占用 2M 的内存，而且这个请求所占用的内存并不会释放，这样下去怕是没两天服务器就要爆内存了。

后来我也想过解决办法，比如用 PM2 这个工具来限制运行的内存，超过就重启 Node 环境，也想过定时重启服务器，再转念一想，我是大爷诶，凭啥要我去迁就辣鸡 Node.JS 的内存管理，你不行那我换一个具有垃圾回收的语言不就好了，那就 Java？好像也不太行，毕竟服务器就那么点硬盘，JDK 和 JRE 不知道要占用我多少空间，再者说来毕竟我可是 「Java 黑」。

那么就归纳一下我的需求：「占用内存小、部署方便、有垃圾回收（不会爆内存）、~不要 Java~」，然后考虑到编译型程序比解释型程序占用的内存更小，所以也就没考虑 Ruby & Python，满足这些要求的好像也只有 Golang 了。

写了那么久的动态类型语言，突然要我写静态类型语言还真是有点不适应。在网上找了个例子，自己捣鼓了一个下午，就写出来了，算是一个勉强遵循「RESTful」风格的 API，开始还有日志功能，后来想想没必要，Nginx 也可以监控端口的访问日志，就删去了。

然后在 Nginx 配置端口

```
1server {2  listen        80;3  server_name    api.itswincer.com;4  location / {5    add_header Content-Type 'application/javascript';6    proxy_pass http://localhost:520;7  }8}
```

而且 Golang 的部署也是很方便，将 \*.go 拷贝就行了。跑了几天，内存占用稳定在 10M 上下。

[该项目](https://github.com/WincerChan/hitokoto)已托管至 GitHub。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA-SS "搭建 SS")搭建 SS

搬瓦工的 SS/SSR 搭建可以说是非常的方便了：

1.  先进入 KiwiVM 面板
2.  在左侧点击 `Shadowsocks Server` 按钮
3.  再点击 `install Shadowsocks Server` 按钮

大约半分钟后，会提示已经安装完成：

![安装](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

安装

再点击 `Go back` 按钮，回到以下界面，再点击 `Start`：![配置](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACwAAAAAAQABAAACAkQBADs=)

配置

SS 服务配置完成了，将以上信息填入 SS 客户端即可使用。

不过由于服务器是在美西，所以无论怎么优化（BBR），延迟都会在 160ms 以上，当然这对浏览网页看视频来说也没有什么影响。

> 注意：**当你使用 VPS 翻墙时，会同时计算上行、下行流量，也就是说如果翻墙使用 1G 流量，其实等于使用了 VPS 的 2G 流量**。

## [](https://blog.itswincer.com/posts/b3085a7/posts/b3085a7/#%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BA%91%E7%AC%94%E8%AE%B0 "搭建私有云笔记")搭建私有云笔记

我最近有在思考私有云笔记的必要性，毕竟有了博客，那云笔记的作用可能就鸡肋了一点。但我还是选择了搭建。我的想法是：博客用于存放、发布一些较正式的文章，而笔记可以休闲一点（类似作文和日记的区别）。

回到正题，目前来说，体验好的云笔记要么需要会员、要么存在诸多功能限制，而我又不想多浪费钱，那么选择一个支持多设备（其实主要是解决手机设备）的同步方案并借助私有的服务器架设自然也就是最好的解决办法了。我选择了 [Nextcloud 作为解决方案](https://blog.itswincer.com/posts/bf0413ac/)，并借助他的 WebDAV 功能作为多端同步工具。

手机端笔记软件选择的是[易码](https://www.coolapk.com/apk/me.tshine.easymark)，支持 Markdown 语法和 WebDAV 同步，电脑端可以选择直接用浏览器访问 Nextcloud，可以在线 Markdown 编辑和预览，当然也可以选择用 Nextcloud 同步至本地文件夹，并用其它编辑器打开就可以了。

> **本文标题：** 一台 VPS 的正确打开方式
> **最后更新：**2018 年 04 月 02 日 \- 14:04
> **本文链接：**[https://blog.itswincer.com/posts/b3085a7/](https://blog.itswincer.com/posts/b3085a7/)
> **本文采用：署名\-非商业性使用\-禁止演绎 4.0 国际** 协议进行许可，阅读 [相关说明](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)


