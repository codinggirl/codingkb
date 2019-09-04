---
title: 前后端分离的身份认证(一)：JSON WEB TOKEN介绍
created: '2019-09-04T09:46:17.608Z'
modified: '2019-09-04T09:46:20.060Z'
---

# 前后端分离的身份认证(一)：JSON WEB TOKEN介绍

*   [jwt](https://mrxf.github.io/tags/jwt/)
*   [前端](https://mrxf.github.io/tags/%E5%89%8D%E7%AB%AF/)

[![](http://cdn.thisjs.com/blog/software-720x380.jpg "安全")](http://cdn.thisjs.com/blog/software-720x380.jpg "安全")

随着前端单页面APP的发展，前后端分离成为了现在开发的一种趋势，用户身份认证，发生了一系列的变化。传统的Cookie, Session验证方式存在跨域、扩展性的限制，Token验证方式成为了一个很好的替代选择。

这是一篇前导文章，之后会发布一系列关于JSON WEB TOKEN的项目实践。因此，这里将自己了解的相关知识和自己的一些观点汇集于此，以供查阅。

# [](#传统验证方式的不足 "传统验证方式的不足")传统验证方式的不足

> 当然，传统验证方式并不是一文不值的，这里只是列出其中的不足，然后使用JSON WEB TOKEN来弥补其中的缺点。

*   **服务端性能消耗** 每次与用户建立会话之后，都会在服务端保存该信息，例如：PHP Session是保存在文件中，而Java Session则是保存在内存中，随着用户量的提升，会大量占用服务器的资源。
*   **限制了分布式部署** 当服务器处于分布式环境下，Session共享问题便随之而出，因此需要单独的服务器资源来解决Session共享问题。
*   **与Restful API的stateless冲突** Restful思想正在逐步推广，而Session则引入了新的“状态”，与Restful思想矛盾。
*   **不方便移动APP的开发** 使用Session验证方式，限制了原生Android，IOS APP的数据交互。
*   **XSS** Session的提交方式，是将Session信息存储在Cookie中，提交到服务器端，因此很容易被客户端注入的javascript代码，截获Cookie信息。
*   **XSRF** 基于Session的验证方式，有可能会被跨站请求伪造。

# [](#JSON-WEB-TOKEN "JSON WEB TOKEN")JSON WEB TOKEN

## [](#简单介绍 "简单介绍")简单介绍

JWT包含3部分数据信息，使用”.”分割，格式示例如下

|

1

 |

hhhhhhh.pppppp.sssss

 |

三部分信息分别为：

`Signature`: 签名

### [](#Header-头信息 "Header 头信息")Header 头信息

Header中一般包含Token类型和哈希算法，例如:

|

1

 |

{"alg":"HS256","typ":"JWT"}

 |

### [](#Payload-有效荷载 "Payload 有效荷载")Payload 有效荷载

Payload中包含声明信息，例如

|

1
2
3
4
5

 |

{
 "username": "admin",
 "iat":1506320911,  // 创建时间
 "exp":1506324511  // 过期时间
}

 |

> **注意：** Payload和Header中的信息是BASE64编码，不是加密，因此不要再payload中添加敏感信息

### [](#Signature-签名 "Signature 签名")Signature 签名

签名用来校验JWT的发送方属实，以及确认消息在传递途中没有被更改。
例如，使用HS256算法，签名将采用如下方式创建：

|

1
2
3
4

 |

HS256(
 base64UrlEncode(header) + "." +
 base64UrlEncode(payload),
 secret)

 |

这里对于jwt的介绍只是简单介绍，详细关于JWT的信息可以参阅[\[2\]](http://www.jianshu.com/p/576dbf44b2ae),[\[3\]](https://github.com/smilingsun/blog/issues/1)这两篇文章。

## [](#JWT的优点 "JWT的优点")JWT的优点

*   **可以实现跨域请求** 因为JWT不依赖于Cookie，它可以添加在请求的`Header`,`body`,`参数`中，因此只要服务器允许跨域请求，那么带有授权Token的客户端，可以任意访问不同服务器下的服务，因此，非常适合SSO单点登录系统。
*   **减少服务器消耗** 服务器在生成Token之后，就将Token返回给客户端，客户端保存Token用于下次请求。服务端不进行储存Token，只验证Token，减少了服务器的消耗。同时，带有Token的请求在请求不同服务时，不用考虑是与哪台服务器生成的Session问题，非常适用于云服务。
*   **通用性** 因为JSON的通用性，所以JWT可以在Nodejs，JAVA，PHP等不同平台使用。

## [](#JWT示意图 "JWT示意图")JWT示意图

[![](http://www.plantuml.com/plantuml/svg/IqmkoIzIU3vbnREExLosjWel-hO_xPyMaf-TcgTWQyi5aL3GjLDuDgVtQTV2fpDNM2aydxdxwTwfN70HDEcNgtfUh6-4A7goV-6pnlKmOsnqaTMzs3_bwUnvOAkWolBoIr8LSbBJIgmKdkoV-tJj3B1aJDxJZWtFvgnvEcS3o8LFzku76eVVXKztB7koemkivlqUe6ksFzlOv1ZWIS39OFNutFfiwePdlUi0 "undefined")](http://www.plantuml.com/plantuml/svg/IqmkoIzIU3vbnREExLosjWel-hO_xPyMaf-TcgTWQyi5aL3GjLDuDgVtQTV2fpDNM2aydxdxwTwfN70HDEcNgtfUh6-4A7goV-6pnlKmOsnqaTMzs3_bwUnvOAkWolBoIr8LSbBJIgmKdkoV-tJj3B1aJDxJZWtFvgnvEcS3o8LFzku76eVVXKztB7koemkivlqUe6ksFzlOv1ZWIS39OFNutFfiwePdlUi0 "undefined")

## [](#安全问题 "安全问题")安全问题

*   Payload中的内容是BASE64编码，如果需要，可以在编码前，对内容进行加密
*   生成签名的密钥除了妥善保存之外，可以使用**动态密钥**，在启动服务时生成密钥，这样就不会被轻易获取

# [](#参考资料 "参考资料")参考资料

\[1\] [前后端分离之JWT用户认证](http://lion1ou.win/2017/01/18/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

\[2\] [什么是 JWT – JSON WEB TOKEN](http://www.jianshu.com/p/576dbf44b2ae)

\[3\] [适用于前后端分离的下一代认证机制 —— JSON Web Token](https://github.com/smilingsun/blog/issues/1)

\[4\] [基于JSON Web Token的无状态账户系统的设计](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2016&filename=XDJS201616018)

\[5\] [JWT认证技术及其在WEB中的应用](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2016&filename=SZJT201602087)

