---
title: koa
created: '2019-09-04T01:42:48.430Z'
modified: '2019-09-20T07:25:27.075Z'
---

# Koa 编程指南

Node 奇数版本属于不稳定版本，偶数版本属于稳定版本。我们建议直接使用最新的偶数版本的 Node。

Node 从 v7.6.0 开始才完全支持async/await，Koa 大量使用了 async、await 函数，我们需要确保自己使用的是 Node 7.6 及以上的版本。

Koa 是一个轻量级的 Web 框架。

## 安装及使用

### 检查 Node.js 版本

查看 node 版本：

```
node -v
```
### 新建 Web 项目

```
mkdir your-project
cd your-project
```
```
npm init
```
```
npm i --save koa
```
### 创建文件
```
// app.js
const Koa = require('koa');
const app = new Koa();

let hello = async ctx => {
    ctx.body = 'hello, world'
}

app.use(hello)

app.listen(3000);
```
启动程序

```
node app.js
```
在浏览器中输入网址打开。
```
open http://127.0.0.1:3000
```
我们会看到浏览器中输出了`hello, world`字样。

## 编辑器及跳水

编辑器与调试
前端使用的编辑器五花八门，主流的有 webstorm 或 phpstorm、Sublime Text、vim等，用于编写 node 应用，推荐使用 webstorm。

支持 cmd 语法，且可以检查依赖模块路径的正确性
错误检查
内置 node 调试服务
内置命令行工具
内置单元测试工具
支持ES6语法
集成了git版本控制
开启 ES6 支持
由于 koa 应用会使用 ES6 的语法，所以我们修改 webstorm js的配置，避免语法误报错：

es6

在 webstorm 中启动 koa 应用
打开 book/app.js ，选择菜单的 "Run"：

debug

点击配置编辑，我们需要调整下配置：

config

留意 node 参数配置，我们需要把 --harmony 配置上。

除了可以配置 node 参数外，还可以配置应用的环境变量。

比如我们配置个 NODE_ENV=local，用于开启本地环境配置：

config

配置完成后，我们就可以使用 webstorm 来启动 koa 应用，菜单 “Run” -> “Run book”，启动成功后：

config

左侧按钮依次是 重启、停止、暂停。

在 webstorm 中调试 koa 应用
打开 book/app.js ，我们在第九行打个断点（点击第九行行号右侧空白处）：

config

点击菜单 “Run” -> “Debug book” ,访问 http://localhost:3000 :

config

就会断在第九行，可以查看变量信息，debug 过程跟浏览器断点调试差不多，可以通过面板的右上角按钮控制debug。

debug 的调试效率远比 console.log() 来的高，推荐使用。

webstorm 也集成了单元测试工具，这等到讲解 koa 应用单元测试时，我们再来讲解。

debug模块的使用
通常我们使用 console.log() 来打印 log 信息，log 一多对排查问题反而形成干扰，同时有些log我们希望只存在于开发环境。

所以，推荐使用 debug 模块，debug 模块可以对log进行分组，而且只有设置了环境变量 DEBUG 的情况下才会输出 log 。

var debug = require('debug')('book');
app.get('/config',function *(){
    var config = this.config;
    debug('env: %s',config.env);
    this.body = config.env;
});
配置环境变量：DEBUG=book ，这样只会输出 book 分组的 log 信息。

DEBUG=book NODE_ENV=local node --harmony app.js
多个环境变量，使用空格隔开。

访问页面后，输出的log是：

book env: local +0ms

如果去掉环境变量 DEBUG=book 将不输出任何信息，这样当我们需要调试某个模块文件时，可以保证只有该模块的 log ，不会被其他模块所干扰。

如果我们想输出所有的调试信息呢？

可以使用 DEBUG=* 。

日志记录
日志是应用不可缺少的部分，但 koa 没有日志记录中间件，我们使用 mini-logger 。

mini-logger 会创建日志记录文件，自动使用日期归档，而且还可以自定义日志分类。

var Logger = require('mini-logger');
var logger = Logger({
    dir: config.logDir,
    categories: [ 'router','model','controller'],
    format: 'YYYY-MM-DD-[{category}][.log]'
});
dir ：指定日志放在哪里
categories ：自定义日志分类
format ：日志文件名格式
记录 Error() 实例：

logger.error(new Error('test'));
使用 error() ，会将出错堆栈信息输出到 xxxx-xx-xx-error.log 文件中。

当定义自定义分类后，会自动注册分类日志方法，比如定义了 categories: ['router'] ：

logger.router('test router');
就有了 logger.router() 方法，并将日志记录到 xxxx-xx-xx-router.log 文件中。

给日志分类，让我们排查线上问题更有效率。

## Koa 对象

koa 一共有3个对象：Context、Request、Response。

Context 对象，表示一次对话的上下文（它包括 Request 和 Response）。

通过修改这个对象的一些内容，可以控制返回给用户的内容。

## 路由

### 原生路由

通过ctx.request.path可以获取用户请求的路径，由此实现简单的路由。

// demos/05.js
const main = ctx => {
  if (ctx.request.path !== '/') {
    ctx.response.type = 'html';
    ctx.response.body = '<a href="/">Index Page</a>';
  } else {
    ctx.response.body = 'Hello World';
  }
}

### koa-route 模块

原生路由用起来不太方便，我们可以使用封装好的koa-route模块。

```
// demos/06.js
const route = require('koa-route');

const about = ctx => {
  ctx.response.type = 'html';
  ctx.response.body = '<a href="/">Index Page</a>';
};

const main = ctx => {
  ctx.response.body = 'Hello World';
};

app.use(route.get('/', main));
app.use(route.get('/about', about));
```


### 路由的基本用法

通过 ctx.path 属性来判断用户请求的路径，可以达到路由作用。

```
app.use(async (ctx,next)=>{
  if (ctx.path === '/') {
    ctx.body = 'we are at home!';
  } else {
    await next;
  }
})

app.use(async (ctx,next)=>{
  if (ctx.path === '/404') {
    ctx.body = 'page not found';
  } else {
    await next;
  }
})
```

每个中间件负责一个路径，如果路径不符合，就传递给下一个中间件。

对于复杂的路由，我们需要专门的程序来处理。比如选择 `koa-router`。

```
var Koa = require('koa');
var Router = require('koa-router');

var app = new Koa();
var router = new Router();

router.get('/', async (ctx, next) =>{
  ctx.body = 'we are at home!';
});

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(3000)
```

路径匹配的时候，不会把查询字符串考虑在内。比如，/index?param=xyz 匹配路径为 /index
HTTP动词方法
Koa-router实例提供一系列动词方法，即一种HTTP动词对应一种方法。

```
router
  .get('/', async (ctx, next) => {
    ctx.body = 'Hello World!';
  })
  .post('/users', async (ctx, next) => {...})
  .put('/users/:id', async (ctx, next) => {...})
  .del('/users/:id', async (ctx, next) => {...})
  .all('/users/:id', async (ctx, next) => {...})
```

router.all()用于表示上述所有的动词方法

```
router.get('/', async (ctx,next) => {
  ctx.body = 'Hello World!';
});
```

上面代码中，router.get方法的第一个参数是根路径，第二个参数是对应的函数方法。
路由参数
我们可以通过ctx.params得到URL参数
1
2
3
4
router.get('/:category/:title', function (ctx, next) {
  console.log(ctx.params);
  // => { category: 'programming', title: 'how-to-node' }
});
支持多个中间件
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
router.get(
  '/users/:id',
  function (ctx, next) {
    return User.findOne(ctx.params.id).then(function(user) {
      ctx.user = user;
      return next();
    });
  },
  function (ctx) {
    console.log(ctx.user);
    // => { id: 17, name: "Alex" }
  }
);
路由前缀
1
2
3
4
5
6
var router = new Router({
  prefix: '/users'
});

router.get('/', ...); // responds to "/users"
router.get('/:id', ...); // responds to "/users/:id"
重构
每个 url 对应一个规则，如果全部放在 app.js 中会造成代码紊乱且难以理解。因此重构整个项目，项目文件结构如下：
1
2
3
4
5
|-controller        # 用于存放路由规则
| |-index.js        # 首页路由规则
| |-user.js         # user路由规则
|-controller.js     # 自动导入controller下的所有路由规则
|-app.js            # 入口文件，用于启动koa服务器
index.js 内容如下
1
2
3
4
5
6
7
const homepage = async (ctx, next) =>{
  ctx.body = 'we are at home!';
}

module.exports = {
    'GET /': homepage
};
当 require(‘controller/index’) 时，会得到一个包含了规则的对象 {'GET /': homepage} 其中，GET 表示 GET 方法 / 表示解析路径，homepage 是针对这个路径所做的操作。解析规则由 controller.js 的 add_rule 方法实现
controller.js 内容如下：
 
const fs = require('fs')
const Router = require('koa-router');
const router = new Router();

// 解析规则 {'GET /': homepage}
function add_rule(router, rule) {
    for (let key in rule) {
        // key = 'GET /' rule = {'GET /': homepage}
        if (key.startsWith('GET ')) {
            let path = key.substring(4);
            router.get(path, rule[key]);
            console.log(`register URL mapping: GET ${path}`);
        } else if (key.startsWith('POST ')) {
            let path = key.substring(5);
            router.post(path, rule[key]);
            console.log(`register URL mapping: POST ${path}`);
        } else {
            console.log(`invalid URL: ${key}`);
        }
    }
}

//自动导入controller文件夹下所有的路由规则
function add_rules(router) {
    // 得到 /controller 所有以js结尾的文件
    let files = fs.readdirSync(__dirname + '/controller');
    let js_files = files.filter((f) => {
        return f.endsWith('.js');
    });

    // 添加规则
    for (let f of js_files) {
        console.log(`process controller: ${f}...`);
        let rule = require(__dirname + '/controller/' + f);
        add_rule(router, rule);
    }
}

module.exports = function () {
    add_rules(router);
    return router.routes();
};
app.js 内容如下:

const Koa = require('koa');
const app = new Koa();

const controller = require('./controller');

//app.use(router.routes())
app.use(controller())
app.listen(3000)


### 将路由放在app.js外

我们通过 `koa-router` 来实现 `app.js` 与路由分离。

安装 `koa-router`：

```
# npm i koa-router --save
```

```
// routes/index.js

const router = require('koa-router')();

router.get('/', async (ctx) => {
  await ctx.render(<#...#>)
});

module.exports = router.routes();
```

```
#!/usr/bin/env node

// app.js

const Koa = require('koa');
const app = new Koa();

let port = 3030

const index = require('./routes/index');
app.use(index);

if (!module.parent) {
    app.listen(port)
}

module.exports = app
```

在 index 路由程序里，我们导出 router.routes() 的返回值，
其实它是一个函数，它包含了 index.js 中声明的所有路由，它作为 koa 中间件使用。

## 静态资源

对于图片、字体、样式表、脚本等静态资源，可以用 `koa-static` 。

const path = require('path');
const serve = require('koa-static');

const static = serve(path.join(__dirname));
app.use(static);

## 重定向

有些场合，服务器需要重定向访问。比如，用户登陆以后，将他重定向到登陆前的页面。
ctx.response.redirect()方法可以发出一个302跳转。

```
// demos/13.js
const redirect = ctx => {
  ctx.response.redirect('/');
  ctx.response.body = '<a href="/">Index Page</a>';
};

app.use(route.get('/redirect', redirect));
```

## 中间件

### Logger 功能

```
const main = ctx => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  ctx.response.body = 'Hello World';
};
```

上一个例子里面的 Logger 功能，可以拆分成一个独立函数（完整代码看这里）。

```
// demos/08.js
const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}
app.use(logger);
```

### 中间件的概念

像上面代码中的logger函数就叫做"中间件"（middleware），因为它处在 HTTP Request 和 HTTP Response 中间，用来实现某种中间功能。
app.use()用来加载中间件。
基本上，Koa 所有的功能都是通过中间件实现的，前面例子里面的main也是中间件。
每个中间件默认接受两个参数，第一个参数是 Context 对象，第二个参数是next函数。只要调用next函数，就可以把执行权转交给下一个中间件。


### 中间件栈

多个中间件会形成一个栈结构（middle stack），以"先进后出"（first-in-last-out）的顺序执行。

```
// demos/09.js
const one = (ctx, next) => {
  console.log('>> one');
  next();
  console.log('<< one');
}

const two = (ctx, next) => {
  console.log('>> two');
  next(); 
  console.log('<< two');
}

const three = (ctx, next) => {
  console.log('>> three');
  next();
  console.log('<< three');
}

app.use(one);
app.use(two);
app.use(three);
```

如果中间件内部没有调用next函数，那么执行权就不会传递下去。

### 异步中间件

如果有异步操作（比如读取数据库），中间件就必须写成 async 函数。

```
// demos/10.js
const fs = require('fs.promised');
const Koa = require('koa');
const app = new Koa();

const main = async function (ctx, next) {
  ctx.response.type = 'html';
  ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
};

app.use(main);
app.listen(3000);
```

上面代码中，fs.readFile是一个异步操作，必须写成await fs.readFile()，然后中间件必须写成 async 函数。

### 中间件的合成

`koa-compose` 模块可以将多个中间件合成为一个。

```
// demos/11.js
const compose = require('koa-compose');

const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}

const main = ctx => {
  ctx.response.body = 'Hello World';
};

const middlewares = compose([logger, main]);
app.use(middlewares);
```

## 错误处理

### 500 错误

如果代码运行过程中发生错误，我们需要把错误信息返回给用户。
HTTP 协定约定这时要返回500状态码。
Koa 提供了ctx.throw()方法，用来抛出错误，ctx.throw(500)就是抛出500错误。

```
// demos/14.js
const main = ctx => {
  ctx.throw(500);
};
```

### 404错误

如果将ctx.response.status设置成404，就相当于ctx.throw(404)，返回404错误。

```
const main = ctx => {
  ctx.response.status = 404;
  ctx.response.body = 'Page Not Found';
};
```

### 处理错误的中间件

为了方便处理错误，最好使用try...catch将其捕获。但是，为每个中间件都写try...catch太麻烦，我们可以让最外层的中间件，负责所有中间件的错误处理。请看下面的例子（完整代码看这里）。
// demos/16.js
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.body = {
      message: err.message
    };
  }
};

const main = ctx => {
  ctx.throw(500);
};

app.use(handler);
app.use(main);
运行这个 demo。
$ node demos/16.js
访问 http://127.0.0.1:3000 ，你会看到一个500页，里面有报错提示 {"message":"Internal Server Error"}。

### error 事件的监听

运行过程中一旦出错，Koa 会触发一个error事件。监听这个事件，也可以处理错误。

```
// demos/17.js
const main = ctx => {
  ctx.throw(500);
};

app.on('error', (err, ctx) =>
  console.error('server error', err);
);
```

### 手动触发 error 事件

需要注意的是，如果错误被try...catch捕获，就不会触发error事件。

这时，必须调用ctx.app.emit()，手动释放error事件，才能让监听函数生效。

```
// demos/18.js`
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.type = 'html';
    ctx.response.body = '<p>Something wrong, please contact administrator.</p>';
    ctx.app.emit('error', err, ctx);
  }
};

const main = ctx => {
  ctx.throw(500);
};

app.on('error', function(err) {
  console.log('logging error ', err.message);
  console.log(err);
});
```

## Web App 的功能

### Cookies

ctx.cookies用来读写 Cookie。

```
const main = function(ctx) {
  const n = Number(ctx.cookies.get('view') || 0) + 1;
  ctx.cookies.set('view', n);
  ctx.response.body = n + ' views';
}
```
使用方法
koa提供了从上下文直接读取、写入cookie的方法

ctx.cookies.get(name, [options]) 读取上下文请求中的cookie
ctx.cookies.set(name, value, [options]) 在上下文中写入cookie
koa2 中操作的cookies是使用了npm的cookies模块，源码在https://github.com/pillarjs/cookies，所以在读写cookie的使用参数与该模块的使用一致。

例子代码
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {

  if ( ctx.url === '/index' ) {
    ctx.cookies.set(
      'cid', 
      'hello world',
      {
        domain: 'localhost',  // 写cookie所在的域名
        path: '/index',       // 写cookie所在的路径
        maxAge: 10 * 60 * 1000, // cookie有效时长
        expires: new Date('2017-02-15'),  // cookie失效时间
        httpOnly: false,  // 是否只用于http请求中获取
        overwrite: false  // 是否允许重写
      }
    )
    ctx.body = 'cookie is ok'
  } else {
    ctx.body = 'hello world' 
  }

})

app.listen(3000, () => {
  console.log('[demo] cookie is starting at port 3000')
})
运行例子

前言
koa2原生功能只提供了cookie的操作，但是没有提供session操作。session就只用自己实现或者通过第三方中间件实现。在koa2中实现session的方案有一下几种

如果session数据量很小，可以直接存在内存中
如果session数据量很大，则需要存储介质存放session数据
数据库存储方案
将session存放在MySQL数据库中
需要用到中间件
koa-session-minimal 适用于koa2 的session中间件，提供存储介质的读写接口 。
koa-mysql-session 为koa-session-minimal中间件提供MySQL数据库的session数据读写操作。
将sessionId和对于的数据存到数据库
将数据库的存储的sessionId存到页面的cookie中
根据cookie的sessionId去获取对于的session信息
快速使用
demo源码

https://github.com/ChenShenhai/koa2-note/blob/master/demo/session/index.js

例子代码
const Koa = require('koa')
const session = require('koa-session-minimal')
const MysqlSession = require('koa-mysql-session')

const app = new Koa()

// 配置存储session信息的mysql
let store = new MysqlSession({
  user: 'root',
  password: 'abc123',
  database: 'koa_demo',
  host: '127.0.0.1',
})

// 存放sessionId的cookie配置
let cookie = {
  maxAge: '', // cookie有效时长
  expires: '',  // cookie失效时间
  path: '', // 写cookie所在的路径
  domain: '', // 写cookie所在的域名
  httpOnly: '', // 是否只用于http请求中获取
  overwrite: '',  // 是否允许重写
  secure: '',
  sameSite: '',
  signed: '',

}

// 使用session中间件
app.use(session({
  key: 'SESSION_ID',
  store: store,
  cookie: cookie
}))

app.use( async ( ctx ) => {

  // 设置session
  if ( ctx.url === '/set' ) {
    ctx.session = {
      user_id: Math.random().toString(36).substr(2),
      count: 0
    }
    ctx.body = ctx.session
  } else if ( ctx.url === '/' ) {

    // 读取session信息
    ctx.session.count = ctx.session.count + 1
    ctx.body = ctx.session
  } 

})

app.listen(3000)
console.log('[demo] session is starting at port 3000')
运行例子
执行命令
node index.js
访问连接设置session
http://localhost:3000/set session-result-01

查看数据库session是否存储
session-result-01

查看cookie中是否种下了sessionId


安装模块
# 安装koa模板使用中间件
npm install --save koa-views

# 安装ejs模板引擎
npm install --save ejs
使用模板引擎
demo源码

https://github.com/ChenShenhai/koa2-note/blob/master/demo/ejs/

文件目录
├── package.json
├── index.js
└── view
    └── index.ejs
./index.js文件
const Koa = require('koa')
const views = require('koa-views')
const path = require('path')
const app = new Koa()

// 加载模板引擎
app.use(views(path.join(__dirname, './view'), {
  extension: 'ejs'
}))

app.use( async ( ctx ) => {
  let title = 'hello koa2'
  await ctx.render('index', {
    title,
  })
})

app.listen(3000)
./view/index.ejs 模板
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1><%= title %></h1>
    <p>EJS Welcome to <%= title %></p>
</body>
</html>




busboy模块
快速开始
安装
npm install --save busboy
模块简介
busboy 模块是用来解析POST请求，node原生req中的文件流。

开始使用
const inspect = require('util').inspect 
const path = require('path')
const fs = require('fs')
const Busboy = require('busboy')

// req 为node原生请求
const busboy = new Busboy({ headers: req.headers })

// ...

// 监听文件解析事件
busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
  console.log(`File [${fieldname}]: filename: ${filename}`)


  // 文件保存到特定路径
  file.pipe(fs.createWriteStream('./upload'))

  // 开始解析文件流
  file.on('data', function(data) {
    console.log(`File [${fieldname}] got ${data.length} bytes`)
  })

  // 解析文件结束
  file.on('end', function() {
    console.log(`File [${fieldname}] Finished`)
  })
})

// 监听请求中的字段
busboy.on('field', function(fieldname, val, fieldnameTruncated, valTruncated) {
  console.log(`Field [${fieldname}]: value: ${inspect(val)}`)
})

// 监听结束事件
busboy.on('finish', function() {
  console.log('Done parsing form!')
  res.writeHead(303, { Connection: 'close', Location: '/' })
  res.end()
})
req.pipe(busboy)
更多模块信息
更多详细API可以访问npm官方文档 https://www.npmjs.com/package/busboy


上传文件简单实现
依赖模块
安装依赖
npm install --save busboy
busboy 是用来解析出请求中文件流
例子源码
demo源码

https://github.com/ChenShenhai/koa2-note/blob/master/demo/upload/

封装上传文件到写入服务的方法
const inspect = require('util').inspect
const path = require('path')
const os = require('os')
const fs = require('fs')
const Busboy = require('busboy')

/**
 * 同步创建文件目录
 * @param  {string} dirname 目录绝对地址
 * @return {boolean}        创建目录结果
 */
function mkdirsSync( dirname ) {
  if (fs.existsSync( dirname )) {
    return true
  } else {
    if (mkdirsSync( path.dirname(dirname)) ) {
      fs.mkdirSync( dirname )
      return true
    }
  }
}

/**
 * 获取上传文件的后缀名
 * @param  {string} fileName 获取上传文件的后缀名
 * @return {string}          文件后缀名
 */
function getSuffixName( fileName ) {
  let nameList = fileName.split('.')
  return nameList[nameList.length - 1]
}

/**
 * 上传文件
 * @param  {object} ctx     koa上下文
 * @param  {object} options 文件上传参数 fileType文件类型， path文件存放路径
 * @return {promise}         
 */
function uploadFile( ctx, options) {
  let req = ctx.req
  let res = ctx.res
  let busboy = new Busboy({headers: req.headers})

  // 获取类型
  let fileType = options.fileType || 'common'
  let filePath = path.join( options.path,  fileType)
  let mkdirResult = mkdirsSync( filePath )

  return new Promise((resolve, reject) => {
    console.log('文件上传中...')
    let result = { 
      success: false,
      formData: {},
    }

    // 解析请求文件事件
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      let fileName = Math.random().toString(16).substr(2) + '.' + getSuffixName(filename)
      let _uploadFilePath = path.join( filePath, fileName )
      let saveTo = path.join(_uploadFilePath)

      // 文件保存到制定路径
      file.pipe(fs.createWriteStream(saveTo))

      // 文件写入事件结束
      file.on('end', function() {
        result.success = true
        result.message = '文件上传成功'

        console.log('文件上传成功！')
        resolve(result)
      })
    })

    // 解析表单中其他字段信息
    busboy.on('field', function(fieldname, val, fieldnameTruncated, valTruncated, encoding, mimetype) {
      console.log('表单字段数据 [' + fieldname + ']: value: ' + inspect(val));
      result.formData[fieldname] = inspect(val);
    });

    // 解析结束事件
    busboy.on('finish', function( ) {
      console.log('文件上结束')
      resolve(result)
    })

    // 解析错误事件
    busboy.on('error', function(err) {
      console.log('文件上出错')
      reject(result)
    })

    req.pipe(busboy)
  })

} 


module.exports =  {
  uploadFile
}
入口文件
const Koa = require('koa')
const path = require('path')
const app = new Koa()
// const bodyParser = require('koa-bodyparser')

const { uploadFile } = require('./util/upload')

// app.use(bodyParser())

app.use( async ( ctx ) => {

  if ( ctx.url === '/' && ctx.method === 'GET' ) {
    // 当GET请求时候返回表单页面
    let html = `
      <h1>koa2 upload demo</h1>
      <form method="POST" action="/upload.json" enctype="multipart/form-data">
        <p>file upload</p>
        <span>picName:</span><input name="picName" type="text" /><br/>
        <input name="file" type="file" /><br/><br/>
        <button type="submit">submit</button>
      </form>
    `
    ctx.body = html

  } else if ( ctx.url === '/upload.json' && ctx.method === 'POST' ) {
    // 上传文件请求处理
    let result = { success: false }
    let serverFilePath = path.join( __dirname, 'upload-files' )

    // 上传文件事件
    result = await uploadFile( ctx, {
      fileType: 'album', // common or album
      path: serverFilePath
    })

    ctx.body = result
  } else {
    // 其他请求显示404
    ctx.body = '<h1>404！！！ o(╯□╰)o</h1>'
  }
})

app.listen(3000, () => {
  console.log('[demo] upload-simple is starting at port 3000')
})


快速上手
demo 地址

https://github.com/ChenShenhai/koa2-note/tree/master/demo/upload-async

源码理解
demo源码目录
.
├── index.js # 后端启动文件
├── node_modules
├── package.json
├── static # 静态资源目录
│   ├── image # 异步上传图片存储目录
│   └── js
│       └── index.js # 上传图片前端js操作
├── util
│   └── upload.js # 后端处理图片流操作
└── view
    └── index.ejs # ejs后端渲染模板
后端代码
入口文件 demo/upload-async/index.js

const Koa = require('koa')
const views = require('koa-views')
const path = require('path')
const convert = require('koa-convert')
const static = require('koa-static')
const { uploadFile } = require('./util/upload')

const app = new Koa()

/**
 * 使用第三方中间件 start 
 */
app.use(views(path.join(__dirname, './view'), {
  extension: 'ejs'
}))

// 静态资源目录对于相对入口文件index.js的路径
const staticPath = './static'
// 由于koa-static目前不支持koa2
// 所以只能用koa-convert封装一下
app.use(convert(static(
  path.join( __dirname,  staticPath)
)))
/**
 * 使用第三方中间件 end 
 */

app.use( async ( ctx ) => {
  if ( ctx.method === 'GET' ) {
    let title = 'upload pic async'
    await ctx.render('index', {
      title,
    })
  } else if ( ctx.url === '/api/picture/upload.json' && ctx.method === 'POST' ) {
    // 上传文件请求处理
    let result = { success: false }
    let serverFilePath = path.join( __dirname, 'static/image' )

    // 上传文件事件
    result = await uploadFile( ctx, {
      fileType: 'album',
      path: serverFilePath
    })
    ctx.body = result
  } else {
    // 其他请求显示404
    ctx.body = '<h1>404！！！ o(╯□╰)o</h1>'
  }

})

app.listen(3000, () => {
  console.log('[demo] upload-pic-async is starting at port 3000')
})
后端上传图片流写操作 入口文件 demo/upload-async/util/upload.js

const inspect = require('util').inspect
const path = require('path')
const os = require('os')
const fs = require('fs')
const Busboy = require('busboy')

/**
 * 同步创建文件目录
 * @param  {string} dirname 目录绝对地址
 * @return {boolean}        创建目录结果
 */
function mkdirsSync( dirname ) {
  if (fs.existsSync( dirname )) {
    return true
  } else {
    if (mkdirsSync( path.dirname(dirname)) ) {
      fs.mkdirSync( dirname )
      return true
    }
  }
}

/**
 * 获取上传文件的后缀名
 * @param  {string} fileName 获取上传文件的后缀名
 * @return {string}          文件后缀名
 */
function getSuffixName( fileName ) {
  let nameList = fileName.split('.')
  return nameList[nameList.length - 1]
}

/**
 * 上传文件
 * @param  {object} ctx     koa上下文
 * @param  {object} options 文件上传参数 fileType文件类型， path文件存放路径
 * @return {promise}         
 */
function uploadFile( ctx, options) {
  let req = ctx.req
  let res = ctx.res
  let busboy = new Busboy({headers: req.headers})

  // 获取类型
  let fileType = options.fileType || 'common'
  let filePath = path.join( options.path,  fileType)
  let mkdirResult = mkdirsSync( filePath )

  return new Promise((resolve, reject) => {
    console.log('文件上传中...')
    let result = { 
      success: false,
      message: '',
      data: null
    }

    // 解析请求文件事件
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      let fileName = Math.random().toString(16).substr(2) + '.' + getSuffixName(filename)
      let _uploadFilePath = path.join( filePath, fileName )
      let saveTo = path.join(_uploadFilePath)

      // 文件保存到制定路径
      file.pipe(fs.createWriteStream(saveTo))

      // 文件写入事件结束
      file.on('end', function() {
        result.success = true
        result.message = '文件上传成功'
        result.data = {
          pictureUrl: `//${ctx.host}/image/${fileType}/${fileName}`
        }
        console.log('文件上传成功！')
        resolve(result)
      })
    })

    // 解析结束事件
    busboy.on('finish', function( ) {
      console.log('文件上结束')
      resolve(result)
    })

    // 解析错误事件
    busboy.on('error', function(err) {
      console.log('文件上出错')
      reject(result)
    })

    req.pipe(busboy)
  })

} 

module.exports =  {
  uploadFile
}
前端代码
<button class="btn" id="J_UploadPictureBtn">上传图片</button>
<hr/>
<p>上传进度<span id="J_UploadProgress">0</span>%</p>
<p>上传结果图片</p>
<div id="J_PicturePreview" class="preview-picture">
</div>
<script src="/js/index.js"></script>
上传操作代码

(function(){

let btn = document.getElementById('J_UploadPictureBtn')
let progressElem = document.getElementById('J_UploadProgress')
let previewElem = document.getElementById('J_PicturePreview')
btn.addEventListener('click', function(){
  uploadAction({
    success: function( result ) {
      console.log( result )
      if ( result && result.success && result.data && result.data.pictureUrl ) {
        previewElem.innerHTML = '<img src="'+ result.data.pictureUrl +'" style="max-width: 100%">'
      }
    },
    progress: function( data ) {
      if ( data && data * 1 > 0 ) {
        progressElem.innerText = data
      }
    }
  })
})


/**
 * 类型判断
 * @type {Object}
 */
let UtilType = {
  isPrototype: function( data ) {
    return Object.prototype.toString.call(data).toLowerCase();
  },

  isJSON: function( data ) {
    return this.isPrototype( data ) === '[object object]';
  },

  isFunction: function( data ) {
    return this.isPrototype( data ) === '[object function]';
  }
}

/**
 * form表单上传请求事件
 * @param  {object} options 请求参数
 */
function requestEvent( options ) {
  try {
    let formData = options.formData
    let xhr = new XMLHttpRequest()
    xhr.onreadystatechange = function() {

      if ( xhr.readyState === 4 && xhr.status === 200 ) {
        options.success(JSON.parse(xhr.responseText))
      } 
    }

    xhr.upload.onprogress = function(evt) {
      let loaded = evt.loaded
      let tot = evt.total
      let per = Math.floor(100 * loaded / tot) 
      options.progress(per)
    }
    xhr.open('post', '/api/picture/upload.json')
    xhr.send(formData)
  } catch ( err ) {
    options.fail(err)
  }
}

/**
 * 上传事件
 * @param  {object} options 上传参数      
 */
function uploadEvent ( options ){
  let file
  let formData = new FormData()
  let input = document.createElement('input')
  input.setAttribute('type', 'file')
  input.setAttribute('name', 'files')

  input.click()
  input.onchange = function () {
    file = input.files[0]
    formData.append('files', file)

    requestEvent({
      formData,
      success: options.success,
      fail: options.fail,
      progress: options.progress
    })  
  }

}

/**
 * 上传操作
 * @param  {object} options 上传参数     
 */
function uploadAction( options ) {
  if ( !UtilType.isJSON( options ) ) {
    console.log( 'upload options is null' )
    return
  }
  let _options = {}
  _options.success = UtilType.isFunction(options.success) ? options.success : function() {}
  _options.fail = UtilType.isFunction(options.fail) ? options.fail : function() {}
  _options.progress = UtilType.isFunction(options.progress) ? options.progress : function() {}

  uploadEvent(_options)
}


})()
运行效果

快速开始
安装MySQL数据库
https://www.mysql.com/downloads/

安装 node.js的mysql模块
npm install --save mysql
模块介绍
mysql模块是node操作MySQL的引擎，可以在node.js环境下对MySQL数据库进行建表，增、删、改、查等操作。

开始使用
创建数据库会话
const mysql      = require('mysql')
const connection = mysql.createConnection({
  host     : '127.0.0.1',   // 数据库地址
  user     : 'root',    // 数据库用户
  password : '123456'   // 数据库密码
  database : 'my_database'  // 选中数据库
})

// 执行sql脚本对数据库进行读写 
connection.query('SELECT * FROM my_table',  (error, results, fields) => {
  if (error) throw error
  // connected! 

  // 结束会话
  connection.release() 
});
注意：一个事件就有一个从开始到结束的过程，数据库会话操作执行完后，就需要关闭掉，以免占用连接资源。

创建数据连接池
一般情况下操作数据库是很复杂的读写过程，不只是一个会话，如果直接用会话操作，就需要每次会话都要配置连接参数。所以这时候就需要连接池管理会话。

const mysql = require('mysql')

// 创建数据池
const pool  = mysql.createPool({
  host     : '127.0.0.1',   // 数据库地址
  user     : 'root',    // 数据库用户
  password : '123456'   // 数据库密码
  database : 'my_database'  // 选中数据库
})

// 在数据池中进行会话操作
pool.getConnection(function(err, connection) {

  connection.query('SELECT * FROM my_table',  (error, results, fields) => {

    // 结束会话
    connection.release();

    // 如果有错误就抛出
    if (error) throw error;
  })
})
更多模块信息
更多详细API可以访问npm官方文档 https://www.npmjs.com/package/mysql


async/await封装使用mysql
前言
由于mysql模块的操作都是异步操作，每次操作的结果都是在回调函数中执行，现在有了async/await，就可以用同步的写法去操作数据库

Promise封装mysql模块
Promise封装 ./async-db
const mysql = require('mysql')
const pool = mysql.createPool({
  host     :  '127.0.0.1',
  user     :  'root',
  password :  '123456',
  database :  'my_database'
})

let query = function( sql, values ) {
  return new Promise(( resolve, reject ) => {
    pool.getConnection(function(err, connection) {
      if (err) {
        reject( err )
      } else {
        connection.query(sql, values, ( err, rows) => {

          if ( err ) {
            reject( err )
          } else {
            resolve( rows )
          }
          connection.release()
        })
      }
    })
  })
}

module.exports = { query }
async/await使用
const { query } = require('./async-db')
async function selectAllData( ) {
  let sql = 'SELECT * FROM my_table'
  let dataList = await query( sql )
  return dataList
}

async function getData() {
  let dataList = await selectAllData()
  console.log( dataList )
}

getData()


建表初始化
前言
通常初始化数据库要建立很多表，特别在项目开发的时候表的格式可能会有些变动，这时候就需要封装对数据库建表初始化的方法，保留项目的sql脚本文件，然后每次需要重新建表，则执行建表初始化程序就行

快速开始
demo源码

https://github.com/ChenShenhai/koa2-note/blob/master/demo/mysql/

源码目录
├── index.js # 程序入口文件
├── node_modules/
├── package.json
├── sql   # sql脚本文件目录
│   ├── data.sql
│   └── user.sql
└── util    # 工具代码
    ├── db.js # 封装的mysql模块方法
    ├── get-sql-content-map.js # 获取sql脚本文件内容
    ├── get-sql-map.js # 获取所有sql脚本文件
    └── walk-file.js # 遍历sql脚本文件
具体流程
       +---------------------------------------------------+
       |                                                   |
       |   +-----------+   +-----------+   +-----------+   |
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
+----------+  遍历sql  +---+ 解析所有sql +---+  执行sql  +------------>
       |   |  目录下的  |   |  文件脚本  |   |   脚本     |   |
+----------+  sql文件   +---+   内容    +---+           +------------>
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
       |   |           |   |           |   |           |   |
       |   +-----------+   +-----------+   +-----------+   |
       |                                                   |
       +---------------------------------------------------+
源码详解
数据库操作文件 ./util/db.js
const mysql = require('mysql')

const pool = mysql.createPool({
  host     :  '127.0.0.1',
  user     :  'root',
  password :  'abc123',
  database :  'koa_demo'
})

let query = function( sql, values ) {

  return new Promise(( resolve, reject ) => {
    pool.getConnection(function(err, connection) {
      if (err) {
        reject( err )
      } else {
        connection.query(sql, values, ( err, rows) => {

          if ( err ) {
            reject( err )
          } else {
            resolve( rows )
          }
          connection.release()
        })
      }
    })
  })

}

module.exports = {
  query
}
获取所有sql脚本内容 ./util/get-sql-content-map.js
const fs = require('fs')
const getSqlMap = require('./get-sql-map')

let sqlContentMap = {}

/**
 * 读取sql文件内容
 * @param  {string} fileName 文件名称
 * @param  {string} path     文件所在的路径
 * @return {string}          脚本文件内容
 */
function getSqlContent( fileName,  path ) {
  let content = fs.readFileSync( path, 'binary' )
  sqlContentMap[ fileName ] = content
}

/**
 * 封装所有sql文件脚本内容
 * @return {object} 
 */
function getSqlContentMap () {
  let sqlMap = getSqlMap()
  for( let key in sqlMap ) {
    getSqlContent( key, sqlMap[key] )
  }

  return sqlContentMap
}

module.exports = getSqlContentMap
获取sql目录详情 ./util/get-sql-map.js
const fs = require('fs')
const walkFile = require('./walk-file')

/**
 * 获取sql目录下的文件目录数据
 * @return {object} 
 */
function getSqlMap () {
  let basePath = __dirname
  basePath = basePath.replace(/\\/g, '\/')

  let pathArr = basePath.split('\/')
  pathArr = pathArr.splice( 0, pathArr.length - 1 )
  basePath = pathArr.join('/') + '/sql/'

  let fileList = walkFile( basePath, 'sql' )
  return fileList
}

module.exports = getSqlMap
遍历目录操作 ./util/walk-file.js
const fs = require('fs')

/**
 * 遍历目录下的文件目录
 * @param  {string} pathResolve  需进行遍历的目录路径
 * @param  {string} mime         遍历文件的后缀名
 * @return {object}              返回遍历后的目录结果
 */
const walkFile = function(  pathResolve , mime ){

  let files = fs.readdirSync( pathResolve )

  let fileList = {}

   for( let [ i, item] of files.entries() ) {
    let itemArr = item.split('\.')

    let itemMime = ( itemArr.length > 1 ) ? itemArr[ itemArr.length - 1 ] : 'undefined'
    let keyName = item + ''
    if( mime === itemMime ) {
      fileList[ item ] =  pathResolve + item
    }
  }

  return fileList
}

module.exports = walkFile
入口文件 ./index.js

const fs = require('fs');
const getSqlContentMap = require('./util/get-sql-content-map');
const { query } = require('./util/db');


// 打印脚本执行日志
const eventLog = function( err , sqlFile, index ) {
  if( err ) {
    console.log(`[ERROR] sql脚本文件: ${sqlFile} 第${index + 1}条脚本 执行失败 o(╯□╰)o ！`)
  } else {
    console.log(`[SUCCESS] sql脚本文件: ${sqlFile} 第${index + 1}条脚本 执行成功 O(∩_∩)O !`)
  }
}

// 获取所有sql脚本内容
let sqlContentMap = getSqlContentMap()

// 执行建表sql脚本
const createAllTables = async () => {
  for( let key in sqlContentMap ) {
    let sqlShell = sqlContentMap[key]
    let sqlShellList = sqlShell.split(';')

    for ( let [ i, shell ] of sqlShellList.entries() ) {
      if ( shell.trim() ) {
        let result = await query( shell )
        if ( result.serverStatus * 1 === 2 ) {
          eventLog( null,  key, i)
        } else {
          eventLog( true,  key, i) 
        }
      }
    }
  }
  console.log('sql脚本执行结束！')
  console.log('请按 ctrl + c 键退出！')

}

createAllTables()
sql脚本文件 ./sql/data.sql
CREATE TABLE   IF NOT EXISTS  `data` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data_info` json DEFAULT NULL,
  `create_time` varchar(20) DEFAULT NULL,
  `modified_time` varchar(20) DEFAULT NULL,
  `level` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
sql脚本文件 ./sql/user.sql
CREATE TABLE   IF NOT EXISTS  `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `email` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `nick` varchar(255) DEFAULT NULL,
  `detail_info` json DEFAULT NULL,
  `create_time` varchar(20) DEFAULT NULL,
  `modified_time` varchar(20) DEFAULT NULL,
  `level` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `user` set email='1@example.com', password='123456';
INSERT INTO `user` set email='2@example.com', password='123456';
INSERT INTO `user` set email='3@example.com', password='123456';
效果
执行脚本
node index.js
执行结果
mysql-init-result-01

查看数据库写入数据

原生koa2实现jsonp
前言
在项目复杂的业务场景，有时候需要在前端跨域获取数据，这时候提供数据的服务就需要提供跨域请求的接口，通常是使用JSONP的方式提供跨域接口。

实现JSONP
demo地址

https://github.com/ChenShenhai/koa2-note/blob/master/demo/jsonp/

具体原理

  // 判断是否为JSONP的请求 
  if ( ctx.method === 'GET' && ctx.url.split('?')[0] === '/getData.jsonp') {
    // 获取jsonp的callback
    let callbackName = ctx.query.callback || 'callback'
    let returnData = {
      success: true,
      data: {
        text: 'this is a jsonp api',
        time: new Date().getTime(),
      }
    } 

    // jsonp的script字符串
    let jsonpStr = `;${callbackName}(${JSON.stringify(returnData)})`

    // 用text/javascript，让请求支持跨域获取
    ctx.type = 'text/javascript'

    // 输出jsonp字符串
    ctx.body = jsonpStr
  }
解析原理
JSONP跨域输出的数据是可执行的JavaScript代码
ctx输出的类型应该是'text/javascript'
ctx输出的内容为可执行的返回数据JavaScript代码字符串
需要有回调函数名callbackName，前端获取后会通过动态执行JavaScript代码字符，获取里面的数据
效果截图
同域访问JSON请求
jsonp-result-01

跨域访问JSON请求
jsonp-result-02

完整demo代码
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {


  // 如果jsonp 的请求为GET
  if ( ctx.method === 'GET' && ctx.url.split('?')[0] === '/getData.jsonp') {

    // 获取jsonp的callback
    let callbackName = ctx.query.callback || 'callback'
    let returnData = {
      success: true,
      data: {
        text: 'this is a jsonp api',
        time: new Date().getTime(),
      }
    }

    // jsonp的script字符串
    let jsonpStr = `;${callbackName}(${JSON.stringify(returnData)})`

    // 用text/javascript，让请求支持跨域获取
    ctx.type = 'text/javascript'

    // 输出jsonp字符串
    ctx.body = jsonpStr

  } else {

    ctx.body = 'hello jsonp'

  }
})

app.listen(3000, () => {
  console.log('[demo] jsonp is starting at port 3000')
})

koa-jsonp中间件
koa.js 官方wiki中也介绍了不少jsonp的中间件 jsonp-wiki

其中koa-jsonp是支持koa2的，使用方式也非常简单，koa-jsonp的官方demo也很容易理解

快速使用
demo地址

https://github.com/ChenShenhai/koa2-note/blob/master/demo/jsonp-use-middleware/

安装
npm install --save koa-jsonp
简单例子
const Koa = require('koa')
const jsonp = require('koa-jsonp')
const app = new Koa()

// 使用中间件
app.use(jsonp())

app.use( async ( ctx ) => {

  let returnData = {
    success: true,
    data: {
      text: 'this is a jsonp api',
      time: new Date().getTime(),
    }
  }

  // 直接输出JSON
  ctx.body = returnData
})

app.listen(3000, () => {
  console.log('[demo] jsonp is starting at port 3000')
})


单元测试
前言
测试是一个项目周期里必不可少的环节，开发者在开发过程中也是无时无刻进行“人工测试”，如果每次修改一点代码，都要牵一发动全身都要手动测试关联接口，这样子是禁锢了生产力。为了解放大部分测试生产力，相关的测试框架应运而生，比较出名的有mocha，karma，jasmine等。虽然框架繁多，但是使用起来都是大同小异。

准备工作
安装测试相关框架
npm install --save-dev mocha chai supertest
mocha 模块是测试框架
chai 模块是用来进行测试结果断言库，比如一个判断 1 + 1 是否等于 2
supertest 模块是http请求测试库，用来请求API接口
测试例子
demo地址

https://github.com/ChenShenhai/koa2-note/blob/master/demo/test-unit/

例子目录
.
├── index.js # api文件
├── package.json
└── test # 测试目录
    └── index.test.js # 测试用例
所需测试demo
const Koa = require('koa')
const app = new Koa()

const server = async ( ctx, next ) => {
  let result = {
    success: true,
    data: null
  }

  if ( ctx.method === 'GET' ) { 
    if ( ctx.url === '/getString.json' ) {
      result.data = 'this is string data'
    } else if ( ctx.url === '/getNumber.json' ) {
      result.data = 123456
    } else {
      result.success = false
    }
    ctx.body = result
    next && next()
  } else if ( ctx.method === 'POST' ) {
    if ( ctx.url === '/postData.json' ) {
      result.data = 'ok'
    } else {
      result.success = false
    }
    ctx.body = result
    next && next()
  } else {
    ctx.body = 'hello world'
    next && next()
  }
}

app.use(server)

module.exports = app

app.listen(3000, () => {
  console.log('[demo] test-unit is starting at port 3000')
})
启动服务后访问接口会看到以下数据

http://localhost:3000/getString.json

test-unit-result-01

开始写测试用例
demo/test-unit/test/index.test.js

const supertest = require('supertest')
const chai = require('chai')
const app = require('./../index')

const expect = chai.expect
const request = supertest( app.listen() )

// 测试套件/组
describe( '开始测试demo的GET请求', ( ) => {

  // 测试用例
  it('测试/getString.json请求', ( done ) => {
      request
        .get('/getString.json')
        .expect(200)
        .end(( err, res ) => {
            // 断言判断结果是否为object类型
            expect(res.body).to.be.an('object')
            expect(res.body.success).to.be.an('boolean')
            expect(res.body.data).to.be.an('string')
            done()
        })
  })
})
执行测试用例
# node.js <= 7.5.x
./node_modules/.bin/mocha  --harmony

# node.js = 7.6.0
./node_modules/.bin/mocha
注意：

如果是全局安装了mocha，可以直接在当前项目目录下执行 mocha --harmony 命令
如果当前node.js版本低于7.6，由于7.5.x以下还直接不支持async/awiar就需要加上--harmony
会自动读取执行命令 ./test 目录下的测用例文件 inde.test.js，并执行。测试结果如下 test-unit-result-03

用例详解
服务入口加载
如果要对一个服务的API接口，进行单元测试，要用supertest加载服务的入口文件

const supertest = require('supertest')
const request = supertest( app.listen() )
测试套件、用例
describe()描述的是一个测试套件
嵌套在describe()的it()是对接口进行自动化测试的测试用例
一个describe()可以包含多个it()
describe( '开始测试demo的GET请求', ( ) => {
  it('测试/getString.json请求', () => {
      // TODO ...
  })
})
supertest封装服务request，是用来请求接口
chai.expect使用来判断测试结果是否与预期一样
chai 断言有很多中方法，这里只是用了数据类型断言

开发debug
快速开始
环境
node环境 8.x +
chrome 60+
启动脚本
调试demo
https://github.com/ChenShenhai/koa2-note/blob/master/demo/start-quick/

node --inspect index.js
指令框显示
指令框就会出现以下字样

Debugger listening on ws://127.0.0.1:9229/4c23c723-5197-4d23-9b90-d473f1164abe
For help see https://nodejs.org/en/docs/inspector
debug-result

访问chrome浏览器调试server
debug-result

打开浏览器调试窗口会看到一个node.js 的小logo

debug-result

打开chrome浏览器的node调试窗口
debug-result

debug-result

注意打开了node的调试窗口后，原来绿色的node按钮会变灰色，同时调试框会显示debug状态

debug-result

debug-result

可以自定义打断点调试了


### 表单

Web 应用离不开处理表单。
本质上，表单就是 POST 方法发送到服务器的键值对。
koa-body模块可以用来从 POST 请求的数据体里面提取键值对。

```
// demos/20.js
const koaBody = require('koa-body');

const main = async function(ctx) {
  const body = ctx.request.body;
  if (!body.name) ctx.throw(400, '.name required');
  ctx.body = { name: body.name };
};

app.use(koaBody());
```

### 文件上传

koa-body模块还可以用来处理文件上传。

```
const os = require('os');
const path = require('path');
const koaBody = require('koa-body');

const main = async function(ctx) {
  const tmpdir = os.tmpdir();
  const filePaths = [];
  const files = ctx.request.body.files || {};

  for (let key in files) {
    const file = files[key];
    const filePath = path.join(tmpdir, file.name);
    const reader = fs.createReadStream(file.path);
    const writer = fs.createWriteStream(filePath);
    reader.pipe(writer);
    filePaths.push(filePath);
  }

  ctx.body = filePaths;
};

app.use(koaBody({ multipart: true }));
```

## 常用中间件

koa-compose
用于将多个中间件合并成一个中间件

const app = require('koa');
const koaCompose = require('koa-compose');
let middlewareA = async (ctx, next) => {
    //do something;
    async next();
}
let middlewareB = async (ctx, next) => {
    //do something;
    async next();
}
app.use(koaCompose[middlewareA, middlewareB]);
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
koa-router
路由处理中间件

const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

router.get('/', (ctx, next) => {
  // ctx.router available
});

app
  .use(router.routes())
  .use(router.allowedMethods());
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
koa-views
页面渲染中间件

var views = require('koa-views');

// Must be used before any router is used
app.use(views(__dirname + '/views', {
  map: {
    html: 'underscore'
  }
}));

app.use(async function (ctx, next) {
  ctx.state = {
    session: this.session,
    title: 'app'
  };

  await ctx.render('user', {
    user: 'John'
  });
});
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
koa-session-redis
session处理中间件

var session = require('koa-session-redis');

var koa = require('koa');
var app = koa();

app.keys = ['some secret hurr'];
app.use(session({
    store: {
      host: process.env.SESSION_PORT_6379_TCP_ADDR || '127.0.0.1',
      port: process.env.SESSION_PORT_6379_TCP_PORT || 6379,
      ttl: 3600,
    },
  },
));

app.use(function *(){
  var n = this.session.views || 0;
  this.session.views = ++n;
  this.body = n + ' views';
})

app.listen(3000);
console.log('listening on port 3000');
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
koa-bodyparser
post请求body处理中间件

var Koa = require('koa');
var bodyParser = require('koa-bodyparser');

var app = new Koa();
app.use(bodyParser());

app.use(async ctx => {
  // the parsed body will store in ctx.request.body
  // if nothing was parsed, body will be an empty object {}
  ctx.body = ctx.request.body;
});

## 单元测试

node单元测试初见
为什么需要单元测试
前面明河使用二章的内容介绍了 koa 的基础使用与环境配置，这一章我们将用几节时间去讲解 node 的单元测试。

每一本 node 书籍都会强调单元测试的重要性，但都把单元测试的章节放在靠后的位置，不利于引起读者的重视。

单元测试的重要就像城市离不开下水道，虽然一般用户感知不到，但一旦没有或不健全，就是灾难，参见天朝的城市...

浏览器端 js 的单元测试，因为前端业务的多变性和对dom的依赖，业务代码的单测一直很难展开，而 node 应用不存在这个问题，node 中没有dom，而且变化会比前端少，稳定性诉求更高。

node 中单元测试容易展开，且成效好。

所以很有必要且应该在第三章讲解单元测试部分的内容，后面章节 demo 的演示，明河会尽量使用单测用例来演示。

node 中的单元测试
mocha 是最好的 node 单元测试框架，使用简单且灵活，是进行 node 单测的首选。

webstorm 已经集成 mocha 。

安装 mocha：

npm install -g mocha    
在 test 目录我们新建一个 demo-spec.js（约定-spec为用例文件） 文件，测试数组的 indexOf() 方法：

var assert = require("assert");
describe('Array', function() {
    describe('#indexOf()', function() {
        it('should return -1 when the value is not present', function() {
            assert.equal(-1, [1,2,3].indexOf(5));
            assert.equal(-1, [1,2,3].indexOf(0));
        });
    });
});
assert 模块是 node 内置的断言库。

describe() 用于定义测试用例组，是可以嵌套的。

it() 是定义具体的测试用例。

assert.equal() 用于判断值是否符合预期。

命令行运行用例 ：

mocha
默认会运行 test 目录下的所有用例文件，输出的信息：

Array
#indexOf()
  ✓ should return -1 when the value is not present
也可以使用 webstorm 来运行 mocha。

点击菜单 “Run”，选择配置编辑，添加个 mocha :

http://gtms03.alicdn.com/tps/i3/TB1L97yHpXXXXaQaXXX0qnIJpXX-1460-708.png

同样可以配置 node 参数，比如增加 --harmony ,点击运行按钮后：

http://gtms02.alicdn.com/tps/i2/TB1Uc3EHpXXXXb4XpXXpWgeTFXX-594-161.png

webstorm 只会显示出错的用例。

除了使用命令行与 webstorm 外还可以借助 npm 运行 测试脚本，在 package.json 中配置：

"scripts": {
    "test": "mocha --harmony"
}
运行命令 npm test ，就会执行 mocha --harmony 。

should断言库
node 内置的断言库 assert ，功能比较弱，不太好用，推荐使用 should ，详细api可以看 should.js。

should 的断言方法注入到 Object.prototype 中，所以断言的风格更符合用户思维习惯，也支持链式调用，跟 jQuery 有点像：

var should = require("should");
describe('Should test', function() {
    it('number', function() {
        (123).should.be.a.Number;
    });
    it('object property', function() {
        var obj = {name:'minghe',email:"minghe36@gmail.com"};
        obj.should.have.property('name','minghe');
        obj.should.have.property('email');
    });
});
(123).should.be.a.Number 判断 123 是否是一个数字，适用于其他类型的判断。

obj.should.have.property('name','minghe') obj 对象是否包含属性 name ，且 name = 'minghe' 。

常用的 api
使用 ok 判断值是否为 true：

it('ok',function(){
    (true).should.be.ok;
})
使用 equal 判断一个值是否符合预期：

it('equal',function(){
    'abc'.should.equal('abc');
});
使用 not 取反：

it('not equal',function(){
    'abc'.should.not.equal('ddd');
});
判断值是否存在：

it('exist',function(){
    var result = {};
    should.exist(result);
})
更多的 api 请看 文档。

mocha异步测试
在 nodejs 中有非常多的异步逻辑，这些异步逻辑该如何测试呢？

var fs = require('fs');
var should = require('should');
describe('fs', function() {
    describe('#readFile()', function() {
        it('should not be null', function(done) {
            fs.readFile('./package.json', 'utf8', function(err,res){
                if (err) throw err;
                res.should.not.equal(null);
                done();
            });
        });
    });
});
关键在于 done 实参，必须在执行完异步后（在异步回调中）执行下 done()，就能捕获到用例。

回调函数 done() 支持接收一个错误：done(err)，用于简化错误处理。


supertest请求测试
在 node 业务应用中，我们经常需要测试路由的可用性，如何处理呢？

可以使用 supertest 模块，supertest 专门用于 http 断言，支持 koa 的 http 请求测试。

为了保证应用的可测试性，我们需要把应用脚本比如 app.js 中的 koa 实例暴露出来：

var koa = require('koa');
var app = koa();
//一堆中间件...
module.exports = app;
用例写法：

var superagent = require('supertest');
var app = require('../app');

function request() {
    return superagent(app.listen());
}

describe('Routes', function () {
    describe('GET /', function () {
        it('should return 200', function (done) {
            request()
                .get('/')
                .expect(200, done);
        });
    });
});
superagent(app.listen()) 会截获 koa 的 http 请求，可以使用 get 、 post 等方法，对请求进行测试。

request()
    .get('/')
    .expect(200, done);
get('/') 即测试首页 get 请求，.expect(200, done) 测试 请求状态码是否为 200 （请求成功），done 是必须传入的，这样请求测试结束后，才能把测试信息推送给mocha处理。

上述测试代码等价于：

request()
    .get('/')
    .expect(200)
    .end(function(err, res){
        if (err) return done(err);
        done();
    });
.end() 回调会在请求完成后触发，可以在回调中对错误进行处理，res 包含完整的请求信息，可以对这些信息进行测试，比如页面输出的内容等。

运行命令 ：

mocha --harmony
留意：测试 koa 的请求必须加--harmony，否者会抛异常。

我们经常需要对 json 接口的数据结构合法性进行测试，如何借助 supertest 实现测试呢？

我们新建个 /api/user/:id 的路由，返回一个用户信息：

app.get('/api/user/:id',function *(){
    var user = {name:'minghe',email:'minghe36@gmail.com'};
    user = JSON.stringify(user);
    this.body = user;
})
测试此路由是否返回正确的数据：

it('should be json',function(done){
    request()
        .get('/api/user/1')
        .expect(200)
        .end(function(err, res){
            if (err) return done(err);
            var text = res.text;
            var json = JSON.parse(text);
            json.should.have.property('email');
            json.should.have.property('minghe');
            done();
        });
})
supertest 很强大，可以设置请求的头信息，使用 set() ：

request()
    .get('/')
    .set('Accept', 'application/json')
    .expect('Content-Type', /json/)    
而 expect() 除了支持状态码测试外，还支持头信息测试：.expect('Content-Type', /json/) 。

测试覆盖率
我们写了测试用例，但如何知道用例的覆盖率呢？

可以使用 istanbul 实现单元测试覆盖率报告，istanbul 功能非常强大，支持与 mocha 的结合。

在应用工程中执行：

npm install --save-dev istanbul
命令比较长，我们将其写入到 package.json 的 script 方便调用：

"scripts": {
    "test": "NODE_ENV=local node --harmony node_modules/.bin/istanbul cover --report html ./node_modules/mocha/bin/_mocha -- 'test/**/*-spec.js'"
}
NODE_ENV=local node --harmony ：以本地环境、es6支持启动 node 应用；
node_modules/.bin/istanbul cover --report html ：调用 istanbul 程序执行 cover 命令（执行覆盖率计算），--report html 生成的报告以 html 的形式；
./node_modules/mocha/bin/_mocha -- 'test/*/-spec.js' ：关联 mocha 测试驱动程序，执行 tes 目录下 所有的后缀是 -spec.js 的用例文件。
运行 npm test ，如果一切正常，将会在工程目录中生成 coverage 目录，可以打开 coverage/index.html 查看覆盖率报告。

## 项目应用

### 创建项目

##

### Redis


什么是 redis ？
./1.jpg

Redis 是一个开源的内存数据库服务器， 相对于传统数据，它快的惊人，是高性能Key-Value存储系统，在商业应用领域常用于缓存用途（二级存储）。

阿里云基于 redis 做了个 KVStore 的云服务产品，推荐了解下，除了贵，其他都很好...

其实人们低估了 redis，除了缓存用途外，redis 一样可以像 mySQL 一样作为主存储，redis 拥有独一无二的数据模型，它的 value 有 5 种不同类型，可以覆盖大部分的数据结构场景，比如我写的 apebook 图书服务 就是使用 redis 作为存储服务。

这一章只会重点讲解 redis 结合 node 常见的几种用法，更详细的 redis 内容，建议大家购买《 redis 实战》。

论 redis 的重要性
商业级别应用，一般都会有个缓存层，用于提高应用的响应速度与减轻服务器压力，缓存的方案非常多，redis 是其中的佼佼者，下面来简单对比下几种在 node 里面常见的缓存方案。

使用内存
将数据缓存到内存是最简单的方案，也是最危险的方案，node 相对而言更容易出现内存泄露的问题，不适宜存储大数据消耗内存，不具有持久化能力，应用一旦重启，cache 就消失。

npm 上有大量内存 cache 模块，推荐的有 memory-cache、super-cache。

不建议在商业级别应用中使用内存缓存数据。

redis
首先 redis 是内存数据库，非常快； 其次 redis 拥有持久化能力，可以保证缓存数据不丢失； 最后 redis 拥有多种数据类型，可以应对更复杂的业务场景。

除了缓存，redis 还可以干什么？
redis超强的扩展性，非常适合存储次要数据，比如页面的 pv；

## 附录

### Koa 前景

随着 ECMAScript 的进化，V8 引擎的不断更新，Node 的发展越来越好。
许多新的功能和编程方式，都极大地提高了开发者的体验。

Koa 紧跟标准，同时赋予开发者极大的定制空间，和极高的性能。

从2018年开始，我们有理由相信，Koa 会超越 Express，成为 Node Web 界排行第一的框架。

### TJ 传说

了解过 TJ 的童鞋都知道，他以惊为天人的代码贡献速度、源源不断的开发热情和巧夺天工的编程模型而推动整个 Node.js/NPM 社区大步迈进，称为大神毫不过分，而大神的脑回路，向来与凡人不同。

关于大神的传说有很多，最有意思的是在国外著名程序员论坛 reddit 上，有人说，TJ 从来就不是一个人，一个人能有这么高效而疯狂的代码产出实在是太让人震惊了，他背后一定是一个团队，因为他从来都不参加技术会议，也不见任何人，而最后 TJ 离开 Node 社区去转向 Go，这种做事方式非常谷歌，所以 TJ 是谷歌的一个招牌，大家众说纷纭，吵的不可开交，不过有一点大家都是达成共识的，那就是非常肯定和感谢他对于 Nodejs 社区的贡献和付出。

### import/export

ECMAScript Modules 在 Node 9 中开始提供，请参阅 Node 文档：

[ECMAScript Modules](https://nodejs.org/dist/latest-v9.x/docs/api/esm.html)

#### 与 require 的对比

能力 | 描述 | require() | import
--- | --- | --- | ---
NODE_PATH | 从NODE_PATH加载依赖模块 | Y | N
cache | 缓存机制 | 可以通过require的API操作缓存 | 自己独立的缓存机制，目前不可访问
path | 引用路径 | 文件路径 | URL格式文件路径，例如import A from './a?v=2017'
extensions | 扩展名机制 | require.extensions | Loader Hooks
natives | 原生模块引用 | 直接支持 | 直接支持
npm | npm模块引用	直接支持 | 需要Loader Hooks
file | 文件(引用)	*.js,*.json等直接支持 | 默认只能是*.mjs，通过Loader Hooks可以自定义配置规则支持*.js,*.json等Node原有支持文件



### 参考资料

[Koa 网站](http://koajs.com/)

[Koa 框架教程](http://www.ruanyifeng.com/blog/2017/08/koa.html)

[Koa2进阶学习笔记](https://chenshenhai.github.io/koa2-note/)

[koa实战](http://book.apebook.org/minghe/koa-action/index.html)

[koa-router](https://www.npmjs.com/package/koa-router)

[ECMAScript Modules](https://nodejs.org/dist/latest-v9.x/docs/api/esm.html)

