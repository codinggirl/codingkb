---
tags: [JWT]
title: jwt
created: '2019-07-02T01:42:07.530Z'
modified: '2019-09-03T10:09:43.199Z'
---

# JSON Web Token

## 认证和授权

##

JWT 是一种用于双方安全传递信息的简洁的、URL 安全的表述性声明规范。

RFC 7519 是它的规范。

## 登录验证流程

- 客户端请求登录
- 验证信息，登录成功。服务端生成 token，返回客户端。
- 客户端接收 token，存在 localStorage、cookie、本地数据库等。
- 以后每次请求服务器授权的 API，都带上 token。
- 服务器端验证 token。

## JWT 标准

JWT 包含 3 个部分：header、payload、signature。

三部分以句点分割。

```
header.payload.signature
```

### header

header 包含两部分，类型和加密算法。

```
{
    "alg": "HS256",
    "typ": "JWT"
}
```

payload:
Payload包含实体，包括一些预定义的如iss（签发者） , exp（过期时间戳） , sub（面向的用户） , aud（接收方） , iat（签发时间）等,自定义字段等。

1
2
3
4
5
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
Signature
签名通过前面的header,payload加上一个secret,最终通过加密算法生成,如:

1
2
var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload); 
HMACSHA256(encodedString, 'secret');
最终的结果如:

1
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJuaW5naGFvLm5ldCIsImV4cCI6IjE0Mzg5NTU0NDUiLCJuYW1lIjoid2FuZ2hhbyIsImFkbWluIjp0cnVlfQ.SwyHTEx_RQppr97g4J5lKXtabJecpejuef8AqKYMAJc
客户端发送Token的方式:
发送Token可以通过异步的请求发送JSON,或者加入请求头，或者加入到URL的查询字符串里;

关于请求头的话，RFC 6750定义了一个Bearer Token的标准，like this:

1
Authorization: Bearer <token>
Node的jwt中间件
上面简略的讲了一下JWT的标准，详细内容可以查看官方文档，接下来谈谈Node中的生成Token,Token的验证，Token过期更新，Token持久化等问题。

采用node 的jwt-simple中间件

生成Token:
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
16
17
18
var moment = require('moment');
var jwt = require("jwt-simple");
...
app.set("jwtTokenSecret","LuoXia");
...
var expires = moment().add(7,'days').valueOf();
var token = jwt.encode({
    iss: 'admin',
    exp: expires,
	user: newUser.name
}, req.app.get('jwtTokenSecret'));
res.json({
    token : token,
    expires: expires,
    user:{
        name:newUser.name
    }
});
这里用到了moment中间件，生成过期时间

验证Token:
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
16
17
18
19
...
var token = req.body.access_token;//也可以是请求头或者查询字符串
if(token){
	try{
	    var decoded = jwt.decode(token,req.app.get('jwtTokenSecret'));
	    if(decoded.exp < Date.now()){
	        res.end('token expired',401);
	        return;
		}
	}catch(err){
	    res.send(401);
	    res.end();
	    return;
	}
	//其他处理
} else {
	res.status(401);
    res.end();
}
更新Token:
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
16
17
18
var decoded = jwt.decode(token,req.app.get('jwtTokenSecret'));
if(decoded.exp < Date.now()){
    var expires = moment().add(7,'days').valueOf();
	var token = jwt.encode({
	    iss: decoded.iss,
	    exp: expires,
		user: decoded.user
	}, req.app.get('jwtTokenSecret'));
	res.json({
	    token : token,
	    expires: expires,
	    user:{
	        name:newUser.name
	    }
	});
} else {
	...
}
持久化Token:
待补充

与session对比
session依赖于cookie,容易受CSRF漏洞的攻击，不够安全。而JWTs则不依赖于Cookie,虽然token也可以储存在cookie，但是只作为储存方式，并没有作为验证手段。
基于JWTs的Token验证方式是无状态的(stateless),自包含的，例如上例中生成token时就可以添加自定义项，这样就减少了许多数据库查询。

Cookie可以通过设置使得在主域名和子域名都能访问到，而如果储存在localStorage下的话，只要是非同域都是不能互相访问到的，解决方案是在包含token的域下设置cookie,这样在其他子域名下就能访问到了。

由于其JWT的无状态性，我们可以很方便的开发一些公用的API，访问公用API只需带上Token，而一些商业性的API服务，可以解析不同的Token来进行计费。

前后端完全分离的尝试:
技术选型:
Vue+express+mongodb

分析整个应用:
SPA,客户端需有自己的一套路由
组件化开发，可复用
数据逻辑管理，Vuex
Node提供REST API
Fetch API异步请求，更好的异步流程控制
基于JWT的Token验证方式
Webpack构建方案
跨域问题
论坛主要功能: 登录/注册,发表文章，获取文章,退出登录

客户端:
在组件渲染之前，异步请求获取用户信息:

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
16
17
18
19
20
21
22
23
24
25
26
27
28
created(){
      var token = localStorage.getItem("token");
      var _this = this;
      var content = JSON.stringify({
            access_token:token
          })
      if(token){
        fetch('http://localhost:3000/user',{
          method:'POST',
          headers: {
                      "Content-Type": "application/json",
                      "Content-Length": content.length.toString(),
                    },
          body: content
        }).then(function(res){
          if(res.ok){
            res.json().then(function(data){
              console.log(data.user.name);
              _this.user = data.user;
            });
          } else {
            _this.user = undefined;
          }
        });
      } else {
        this.user = undefined;
      }
}
登录

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
16
17
18
19
20
21
22
23
24
25
26
handleLog(){
    let _this = this;
    let content = JSON.stringify({
            name:_this.name,
            password:_this.password
        });
    fetch('http://localhost:3000/log',{
        method:'POST',
        headers: {
            "Content-Type": "application/json",
            "Content-Length": content.length.toString(),
        },
        body: content
        }).then(function(res){
            if(res.ok){
                res.json().then(function(data){
                localStorage.setItem("token",data.token);
                _this.user = data.user;
                _this.$dispatch('log',data.user);
                _this.$router.go('/');
                });
            } else {
                _this.user = undefined;
            }
        });
}
注册与登录类似

退出登录

1
2
3
4
5
6
logOut(e){
    e.preventDefault();
    localStorage.removeItem("token");
    this.$dispatch("logOut");
    this.$router.go("/");
}
发表文章

需带上Token

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
16
17
18
19
20
21
22
23
handlePost(){
    let _this = this;
    let content = JSON.stringify({
            access_token:localStorage.getItem("token"),
            title:_this.title,
            content:_this.content
        });
    fetch('http://localhost:3000/post',{
        method:'POST',
        headers: {
            "Content-Type": "application/json",
            "Content-Length": content.length.toString(),
        },
        body: content
        }).then(function(res){
            if(res.ok){  
                _this.$router.go('/');
                window.location.reload();
            } else {
               console.log("发表文章失败")
            }
        });
}
路由权限访问限制

对于没有登录/注册的，不允许访问/post路由;

对于已经登录的不允许访问/log和/reg路由;

服务端
登录/注册成功生成Token的API

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
var expires = moment().add(7,'days').valueOf();
var token = jwt.encode({
    iss: 'admin',
    exp: expires,
	user: newUser.name
}, req.app.get('jwtTokenSecret'));
res.status(200);
res.json({
    token : token,
    expires: expires,
    user:{
            name:user.name
        }
});
根据Token提供User信息的API

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
16
17
18
19
20
var token = req.body.access_token;
if(token){
    try{
        var decoded = jwt.decode(token,req.app.get('jwtTokenSecret'));
        if(decoded.exp < Date.now()){
            res.end('token expired',401);
        }
        //console.log(decoded)
        res.status(200);
        res.json({
            user:{
                name:decoded.user
            }
            
        });
    } catch(err){
        res.status(401);
        res.send('no token');
    }
}
验证Token的发布文章API

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
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
var token = req.body.access_token;
    if(token){
        try{
            var decoded = jwt.decode(token,req.app.get('jwtTokenSecret'));
            if(decoded.exp < Date.now()){
                res.end('token expired',401);
                return;
            }
        }catch(err){
            res.send(401);
            res.end();
            return;
        }
        var newPost = new Post({
            name:decoded.user,
            title:req.body.title,
            content:req.body.content
        });
        console.log(newPost);
        newPost.save(function(err,post){
            if(err){
                console.log("发表文章失败");
                res.status(500);
                res.send({error:1});
            } else {
                console.log('发表文章成功');
                res.status(200);
                res.end();
            }
            //console.log(post);
        });
    } else{
        res.status(401);
        res.end();
    }
跨域设置

由于要在客户端服weppack sever中访问API,需解决跨域限制:

1
2
3
4
5
6
app.all("*",function(req,res,next){
  res.header("Access-Control-Allow-Origin", "*");
  res.header('Access-Control-Allow-Headers', 'Content-Type, Content-Length, Authorization, Accept, X-Requested-With');
  res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
  next();
});
总结
JWT是一种OAuth2.0标准的Access_Token的具体实现方案，可以通过Authorization Bearer,url查询字符串，异步请求的req.body等方式向服务端发送JWT token, 其不依赖于Cookie,有效避免了CSRF攻击，其无状态性可以方便我们编写公共API，此种方案，更加符合RESTFUL API设计理念,在前后端分离实践中发挥着重要的作用。


JWT 适用于无状态的 REST API。


不需要刷新 JWT。

## JWT 优点

- 跨语言支持
- JWT 可以在自身存储业务需要的非敏感信息
- 字节占用小，便于传输
- 易于服务器端应用的扩展

## 问题

### token 泄漏了怎么办

- 传输层

使用 HTTPS。

- 客户端

### 重放攻击怎么办

- 配合其他手段

### 续签问题

- 每次请求，重新生成 token。开销大。
- token 有效期设置到半夜。
- 客户端定时刷新。需要注意老的 token 的使用问题。可以使用 access token 和 refresh token。

### 用户注销后 token 还能继续使用的问题

- 验证注销的 token

## JWT 续期

用户频繁使用应用程序时，频繁要求用户登录是不太好的。

延长过期时间会创建一个新的令牌，同时旧的令牌仍然有效，知道它过期。

在每次请求时都生成一个新的令牌，是比较愚蠢的做法。当多个令牌同时有效时，还会有安全问题。

### Web 应用程序

在令牌过期之前刷新令牌。

将令牌过期时间设置为一周。在每次用户打开 Web 应用程序并每隔一小时刷新令牌。
如果用户超过一周没有打开过应用程序，那他们需要再次登录。

要刷新令牌，API 需要一个新的端点，它接收一个有效的、没有过期的 JWT，并返回与新的到期字段相同的前面的 JWT。

之后，Web 应用程序会将令牌存储在某处。

### 移动/本地应用程序

大多数本地应用的登录有且仅有一次。

这时，令牌将永远不会过期。

我们可以使用设备的名称，用户可以去撤销对该设备的访问。

我们可以使用随机字符串来处理针对更改密码等事件的处理。

