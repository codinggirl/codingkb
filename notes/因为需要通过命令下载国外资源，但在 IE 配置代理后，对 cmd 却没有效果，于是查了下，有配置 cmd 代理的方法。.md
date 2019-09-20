---
title: 因为需要通过命令下载国外资源，但在 IE 配置代理后，对 cmd 却没有效果，于是查了下，有配置 cmd 代理的方法。
created: '2019-09-12T08:56:45.735Z'
modified: '2019-09-12T08:56:49.100Z'
---

因为需要通过命令下载国外资源，但在 IE 配置代理后，对 cmd 却没有效果，于是查了下，有配置 cmd 代理的方法。

# Windows[](#windows)

通过设置环境变量来配置代理，一种方式是直接在系统设置中配置（这个就不解释了），另一种方式是在需要时通过 `set` 命令临时设置。

控制代理的环境变量分别是 http\_proxy、http\_proxy\_user、http\_proxy\_pass，不区分大小写，分别代表代理地址（应是 [http://ip:port](http://ip:port) 的形式）、代理用户名、代理密码，一般情况下只需要配置 http\_proxy 即可（其余两个参数暂无条件测试，是否有作用未知），参数格式大致如下所示。

```txt
http_proxy=http://localhost:1080
http_proxy_user=zhangsan
http_proxy_pass=lisi

```

通过 `set` 命令的形式大致如下所示。

```
#设置参数
set http_proxy=http://localhost:1080
set http_proxy_user=zhangsan
set http_proxy_pass=lisi

#删除参数
set http_proxy=
set http_proxy_user=
set http_proxy_pass=

```

另外经测试还有 https\_proxy 环境变量可配置，用于配置 https 的代理，如果未配置则将使用 http\_proxy 的配置。据此可推测有 https\_proxy\_user 等参数。

# Linux[](#linux)

因目前没有环境测试，故以下结论仅根据网上资料整理并推测所得，仅做记录和供参考，详见参考资料。

据资料得，Linux 配置方式与 Windows 相似，仅命令及配置方式有所不同。

可配置的环境变量名分别为 http\_proxy、https\_proxy、ftp\_proxy、no\_proxy，分别是配置 http 代理、https 代理、ftp 代理、不使用代理的地址，参数格式大致如下所示（正确性有待考察，可能需要加 http:// 前缀），no\_proxy 较特殊。

```
http_proxy=192.168.10.91:3128
https_proxy=192.168.10.91:3128
ftp_proxy=192.168.10.91:3128
no_proxy="127.0.0.1, localhost, 172.26.*, 172.25.6.66, 192.168.*"

```

在linux下也有两种配置方式，一是需要在相关系统文件中配置，二是通过 `export` 命令临时设置，这里不做详细介绍。

