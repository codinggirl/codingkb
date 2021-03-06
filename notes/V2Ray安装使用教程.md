---
title: V2Ray安装使用教程
created: '2019-09-05T01:23:21.554Z'
modified: '2019-09-05T01:23:23.587Z'
---

# [V2Ray安装使用教程](https://www.williamlong.info/archives/5724.html)

2019\-6\-3 21:26:23 | 作者: 月光 | 分类: [软件应用](https://www.williamlong.info/cat/software.html "软件应用") | 评论: 2 | 浏览: 20707

[![代理服务器](https://www.williamlong.info/logo/Proxy.gif)](https://www.williamlong.info/tag/Proxy.html)

　　V2Ray是个非常好用的上网工具，但是其安装和配置却比较复杂，客户端也不太完善，因此相对来说不太流行，这里就介绍一个较为简单的V2Ray一键安装脚本的使用方法，供大家参考。

　　推荐使用CentOS 7，并安装[BBR](https://www.williamlong.info/archives/5586.html)优化。

　　之后使用root用户登陆，并执行下面的语句：

> bash <(curl \-s \-L https://git.io/v2ray.sh)

　　之后的菜单是安装和卸载，选择安装后，会出现多个安装协议，例如TCP、TCP\_HTTP、WebSocket、WebSocket + TLS，前三个都很简单，只要默认按键选择即可安装好，但第四个WebSocket + TLS相对来说较为复杂，这里详细介绍一下。

　　选择之前，用户需要先购买一个域名，有很多便宜的gTLD域名可以购买，例如top、xyz、club等域名，价格都只有1美元左右，阿里云那里最便宜的域名是club域名，第一年是5元钱，用完一年不用续费，可以用5元再购买个新的。

　　域名购买好了之后，新建立一个随机的子域名，将子域名解析到CentOS服务器的IP地址，之后还可以注册一个cloudflare账号，这个选项是备选，只用于隐藏服务器IP，使用cloudflare会导致速度降低，一开始不建议用。

　　然后，在菜单里选4：WebSocket + TLS，然后选择自动配置TLS，不安装shadowsocks，其他都默认选项，之后系统会自动安装一个Web服务器Candy，并自动申请和续期域名的SSL证书并部署，之后使用v2ray url命令获得vmess URL链接，即可在V2Ray客户端导入。

　　这里多说一点，如果我们不想安装Candy，想用Apache、Nginx等其他Web服务器，那么在安装的时候就选择：不自动配置TLS，之后还需要手动配置SSL，建议安装一个宝塔面板来配置会简单一些。这里假设v2ray的端口号是8888，伪装目录是test，那么，在Apache服务器配置文件里加入以下内容：

> RewriteEngine On
> RewriteCond %{HTTP:Upgrade} =websocket \[NC\]
> RewriteRule /test/(.\*)           ws://127.0.0.1:8888/$1 \[P,L\]
> RewriteCond %{HTTP:Upgrade} !=websocket \[NC\]
> RewriteRule /test/(.\*)           http://127.0.0.1:8888/$1 \[P,L\]

　　在Nginx服务器配置文件里加入以下内容：

> location /test/ {
>     proxy\_pass http://localhost:8888/;
>     proxy\_http\_version 1.1;
>     proxy\_set\_header Upgrade $http\_upgrade;
>     proxy\_set\_header Connection "upgrade";
>     proxy\_set\_header Host $http\_host;
> }

　　对于Windows系统来说，V2Ray客户端建议使用v2rayN，下载地址是[这里](https://github.com/2dust/v2rayN/releases)，下载v2rayN\-Core.zip即可。

　　下载后，将其解压缩到一个文件夹，选择“服务器”\-“从剪贴板导入批量URL”来导入之前复制的vmess URL链接，然后选择“启用HTTP代理”即可。

　　iOS的V2Ray客户端可以使用美国应用商店的Shadowrocket，价格较为便宜。

　　附：上面的一键安装脚本是第三方脚本，有可能会有一些安全风险，[v2ray](https://www.v2ray.com/)官方的安装脚本命令是bash <(curl \-L \-s https://install.direct/go.sh)，注重安全的可以用这个。

