---
tags: [GitHub]
title: 利用frp轻松实现内网穿透
created: '2019-09-04T08:13:20.272Z'
modified: '2019-09-04T08:13:30.155Z'
---

## 利用frp轻松实现内网穿透

发表于 2018\-09\-01 | 分类于 [服务器技术](https://blog.yiranzai.cn/categories/%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%8A%80%E6%9C%AF/) | [评论数： 0](https://blog.yiranzai.cn/posts/26842/#comments) | 阅读次数： 227

# [](#frp "frp")frp

[README](https://github.com/fatedier/frp/blob/master/README.md) | [中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)

frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp, http, https 协议。

## [](#frp-的作用 "frp 的作用")frp 的作用

*   利用处于内网或防火墙后的机器，对外网环境提供 http 或 https 服务。
*   对于 http, https 服务支持基于域名的虚拟主机，支持自定义域名绑定，使多个域名可以共用一个80端口。
*   利用处于内网或防火墙后的机器，对外网环境提供 tcp 和 udp 服务，例如在家里通过 ssh 访问处于公司内网环境内的主机。

## [](#架构 "架构")架构

![architecture](https://github.com/fatedier/frp/raw/master/doc/pic/architecture.png)

## [](#使用示例 "使用示例")使用示例

### [](#准备工作 "准备工作")准备工作

1.  假设公网服务器IP为x.x.x.x；

    复制

    |

    1
    2
    3
    4
    5
    6

     |

    $ lsb\_release \-a
    LSB Version:    :core\-4.1\-amd64:core\-4.1\-noarch:cxx\-4.1\-amd64:cxx\-4.1\-noarch:desktop\-4.1\-amd64:desktop\-4.1\-noarch:languages\-4.1\-amd64:languages\-4.1\-noarch:printing\-4.1\-amd64:printing\-4.1\-noarch
    Distributor ID:    CentOS
    Description:    CentOS Linux release 7.5.1804 (Core)
    Release:    7.5.1804
    Codename:    Core

     |

2.  查看内网服务器架构

    复制

    |

    1
    2
    3
    4
    5
    6

     |

    $ lsb\_release \-a
    LSB Version:    :core\-4.1\-amd64:core\-4.1\-noarch:cxx\-4.1\-amd64:cxx\-4.1\-noarch:desktop\-4.1\-amd64:desktop\-4.1\-noarch:languages\-4.1\-amd64:languages\-4.1\-noarch:printing\-4.1\-amd64:printing\-4.1\-noarch
    Distributor ID:    CentOS
    Description:    CentOS Linux release 7.5.1804 (Core)
    Release:    7.5.1804
    Codename:    Core

     |

3.  创建新用户

    为了安全，公网服务器应创建新用户用于启用`frp`服务

    复制

    |

    1
    2

     |

    $ adduser frp
    $ cd /home/frp/

     |

    假设内网服务器用户为`abc`

4.  下载安装

    根据对应的操作系统及架构，从 [Release](https://github.com/fatedier/frp/releases) 页面下载最新版本的程序。
    写下这篇文章时，最新版本为`v0.21.0`

    复制

    |

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15

     |

    $ wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp\_0.21.0\_linux\_amd64.tar.gz
    $ tar \-zxvf frp\_0.21.0\_linux\_amd64.tar.gz
    $ mv frp\_0.21.0\_linux\_amd64 frp
    $ cd frp
    $ tree .
    .
    ├── frpc            //放入内网服务器
    ├── frpc\_full.ini
    ├── frpc.ini        //放入内网服务器
    ├── frps            //放入公网服务器
    ├── frps\_full.ini
    ├── frps.ini        //放入公网服务器
    └── LICENSE

    0 directories, 7 files

     |

    将 **frps** 及 **frps.ini** 放到具有公网 IP 的机器上。

    将 **frpc** 及 **frpc.ini** 放到处于内网环境的机器上。

5.  创建服务

    公网

    复制

    |

    1
    2
    3
    4
    5

     |

    $ chmod +x frps
    $ ln \-s /home/frp/frp/frps /usr/local/bin/frps
    $ vim frpsd
    nohup frps \-c /home/frp/frp/frps.ini >> /home/frp/frp/frps.log 2>&1 &
    $ ln \-s /home/frp/frp/frpsd /usr/local/bin/frpsd

     |

    内网

    复制

    |

    1
    2
    3
    4
    5

     |

    $ chmod +x frpc
    $ ln \-s /home/abc/frp/frpc /usr/local/bin/frpc
    $ vim frpcd
    nohup frpc \-c /home/abc/frp/frpc.ini >> /home/abc/frp/frpc.log 2>&1 &
    $ ln \-s /home/abc/frp/frpcd /usr/local/bin/frpcd

     |

至此，准备完毕

### [](#通过-ssh-访问公司内网机器 "通过 ssh 访问公司内网机器")通过 ssh 访问公司内网机器

1.  修改 frps.ini 文件，这里使用了最简化的配置：

    复制

    |

    1
    2
    3

     |

    \# frps.ini
    \[common\]
    bind\_port = 7000

     |

2.  启动 frps：

    复制

    |

    1

     |

    $ frpsd

     |

3.  修改 frpc.ini 文件，假设 frps 所在服务器的公网 IP 为 x.x.x.x；

    复制

    |

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10

     |

    \# frpc.ini
    \[common\]
    server\_addr = x.x.x.x
    server\_port = 7000

    \[ssh\]
    type = tcp
    local\_ip = 127.0.0.1
    local\_port = 22
    remote\_port = 6000

     |

4.  启动 frpc：

    复制

    |

    1

     |

    $ frpcd

     |

5.  通过 ssh 访问内网机器，假设用户名为 abc：

    复制

    |

    1

     |

    $ ssh \-oPort=6000 abc@x.x.x.x

     |

### [](#通过自定义域名访问部署于内网的-web-服务 "通过自定义域名访问部署于内网的 web 服务")通过自定义域名访问部署于内网的 web 服务

有时想要让其他人通过域名访问或者测试我们在本地搭建的 web 服务，但是由于本地机器没有公网 IP，无法将域名解析到本地的机器，通过 frp 就可以实现这一功能，以下示例为 http 服务，https 服务配置方法相同， vhost\_http\_port 替换为 vhost\_https\_port， type 设置为 https 即可。

1.  修改 frps.ini 文件，设置 http 访问端口为 8080：

    复制

    |

    1
    2
    3
    4

     |

    \# frps.ini
    \[common\]
    bind\_port = 7000
    vhost\_http\_port = 8080

     |

2.  启动 frps；

    复制

    |

    1

     |

    $ frpsd

     |

3.  修改 frpc.ini 文件，假设 frps 所在的服务器的 IP 为 x.x.x.x，local\_port 为本地机器上 web 服务对应的端口, 绑定自定义域名 `www.yourdomain.com`:

    复制

    |

    1
    2
    3
    4
    5
    6
    7
    8
    9

     |

    \# frpc.ini
    \[common\]
    server\_addr = x.x.x.x
    server\_port = 7000

    \[web\]
    type = http
    local\_port = 80
    custom\_domains = www.yourdomain.com

     |

4.  启动 frpc：

    复制

    |

    1

     |

    $ frpcd

     |

5.  将 `www.yourdomain.com` 的域名 A 记录解析到 IP `x.x.x.x`，如果服务器已经有对应的域名，也可以将 CNAME 记录解析到服务器原先的域名。

6.  通过浏览器访问 `http://www.yourdomain.com:8080` 即可访问到处于内网机器上的 web 服务。

7.  如果想要隐去端口，可以用`Nginx`做一个反向代理

    复制

    |

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11

     |

    server {
     listen       80;
     server\_name  www.yourdomain.com;

     location / {
     proxy\_pass http://127.0.0.1:8080;
     proxy\_set\_header   Host             $host;
     proxy\_set\_header   X\-Real\-IP        $remote\_addr;
     proxy\_set\_header   X\-Forwarded\-For  $proxy\_add\_x\_forwarded\_for;
     }
    }

     |

### [](#转发-DNS-查询请求 "转发 DNS 查询请求")转发 DNS 查询请求

DNS 查询请求通常使用 UDP 协议，frp 支持对内网 UDP 服务的穿透，配置方式和 TCP 基本一致。

1.  修改 frps.ini 文件：

    复制

    |

    1
    2
    3

     |

    \# frps.ini
    \[common\]
    bind\_port = 7000

     |

2.  启动 frps：

    复制

    |

    1

     |

    $ frpsd

     |

3.  修改 frpc.ini 文件，设置 frps 所在服务器的 IP 为 x.x.x.x，转发到 Google 的 DNS 查询服务器 `8.8.8.8` 的 udp 53 端口：

    复制

    |

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10

     |

    \# frpc.ini
    \[common\]
    server\_addr = x.x.x.x
    server\_port = 7000

    \[dns\]
    type = udp
    local\_ip = 8.8.8.8
    local\_port = 53
    remote\_port = 6000

     |

4.  启动 frpc：

    复制

    |

    1

     |

    $ frpcd

     |

5.  通过 dig 测试 UDP 包转发是否成功，预期会返回 `www.google.com` 域名的解析结果：

    复制

    |

    1

     |

    $ dig @x.x.x.x \-p 6000 www.google.com

     |

## [](#完 "完")完


[利用frp轻松实现内网穿透 | 一冉再的博客](https://blog.yiranzai.cn/posts/26842/)

