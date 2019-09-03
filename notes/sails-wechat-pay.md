---
title: "在 Sails 中正确处理微信支付的服务通知信息"
createdAt: "2018-02-04"
updatedAt: "2018-02-04"
---

Sails 默认的请求解析中，不包含对xml类型请求体的解析。这给处理诸如微信支付结果通知等问题造成了一定困难。

我们可以通过 `express-xml-bodyparser` 来处理 xml 消息，用 `skipper` 来处理默认的消息。

## 安装依赖项目

安装依赖。

```
npm i express-xml-bodyparser --save
```

```
npm install skipper --save
```

## 修改配置文件

我们修改配置文件 `/config/http.js` 中 `middleware` 部分，修改默认的 `bodyParser` 。

下面给出一个例子

```
bodyParser: (function (opts) {
        var xmlParser = require('express-xml-bodyparser')(opts)
        var skipper = require('skipper')(opts)
        return function (req, res, next) {
            if (req.headers && (req.headers['content-type'] == 'text/xml' ||
                req.headers['content-type'] == 'application/xml')) {
                return xmlParser(req, res, next)
            }
            return skipper(req, res, next)
        }
    })({strict: true})
```

## 参考

[Sailsjs+wechat(解决sailsjs无法处理微信消息的问题)](https://www.jianshu.com/p/59eabca88dd9)
