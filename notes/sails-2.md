Sails.js：基于Express的Web快速开发框架
JULY 4, 2014META-4
如今一说到实时程序开发，Websocket一定是避不开的，它向我们提供了继Ajax和Comet之后的一种新型的通信方式，有着资源消耗少、实时性高等优点，这恰好也是实时程序的基本要求。NodeJS，作为一个新兴的平台，恰好契合这些要求，为我们提供了诸多解决方案，其中不乏精品。脍炙人口的当属socket.io，然而这玩意儿偶尔当点佐料还是可以的，要是直接拿它来写Web，可就略麻烦啦，连个room概念都没有，数个用户数还得自己来（SignalR傲娇地笑了），那么有没有为Web量身定制的框架呢？

把实时融入到Web站点的每个角落，这着实是一个令人想想都要激动得蹦跶的点子，而且专门有人这样做了，产物便是Meteor。前端写好模板，后端输出数据，剩下的就不用你操心啦！而且，你在前端可以直接对数据库进行操作噢，简直是融前端、后端于一体了，如果权限设置得好的话，这框架好得无以复加啊。可惜其前端模板与框架耦合得太紧，用起来还是颇不爽的。作为AngularJS的死粉，只好另寻高招。

第一次看到Sails.js大概是好几个月以前啦，那时候对它确实没来电——何必要遵循它MVC的路子而放弃Express本身的灵活性呢？然而当项目里模型逐渐多起来，维护越来越困难的时候，当你迫切需要一个ORM框架（Sequalize、Mongoose云云），需要一个RESTful工具（restify云云），需要一个实时框架（socket.io云云）又需要一个好的项目组织架构的时候，Sails.js必将让抓耳挠腮的你长舒一口气——噢，原来你一直在这里！

首先通过npm安装Sails.js，测试的话可以安装beta版或edge版的，这里选择beta版。目前最新的是v0.10.0-rc8，v0.9部件不支持ORM中模型间的association，v0.10还有其它好多改进。

1
npm install -g sails@beta
我们先看看我们得到了什么。

Sails.js Directory Structure

 

Sails.js将所有代码都按逻辑分门别类了，由上至下，其内容分别是：

.tmp：最后发布时的文件，生产环境下都是自动minify好的哦，后面其实是grunt。

api：里面主要包含models、controllers、policies、services，前两个不用说，第三个相当于一个权限的guard，写法和中间件一样啦，通过就回调，最后一个service想到与AngularJS里面的factory或service，全局性的。

assets：顾名思义就是放css、js什么的地方啦。

config：好多配置文件都在这儿，比如是否允许Sails.js自动对外开放CRUD的RESTful接口，这在开发的时候很好用哈——顺便提一下，Sails.js自带的interactive console也真是非常顺手，倒挺像node-webkit，可以在console里直接执行NodeJS语句。

tasks：指的是grunt的tasks，修改grunt流程的话文件最好放这儿。

views：不多说了，视图文件，默认ejs渲染，也可以指定其它的。

 

这样清晰的目录结构带来的好处就是，Sails.js能够自动在后面连接你的models、controllers云云，你就不需要为了一个model去require(‘../models/foo’)了，当然，自己安装的第三方包还是需要自己动手require的。

这样，我们先来动手写一个controller吧。

1
sails generate controller foo
这样，Sails.js就帮我们自动建立好了一个controller文档噢，就放在api/controllers里，叫做FooController.js。

01
/**
02
 * FooController
03
 *
04
 * @description :: Server-side logic for managing foos
05
 * @help        :: See http://links.sailsjs.org/docs/controllers
06
 */
07
 
08
module.exports = {
09
 
10
};
里面还是空的呜呜，让我们往里面填点东西吧~

1
module.exports = {
2
  hello: function (req, res, next) {
3
    res.end('world');
4
  }
5
};
启动一下服务器，命令就是“扬帆”喏！

1
sails lift
Sails lift访问一下http://localhost:1337/foo/hello，就会返回“world”噢。这里的点在于，Sails.js默认会帮你把controller映射到routes上，当然你也可以在config/routes.js中自定义routes，非常简单。

那说好的RESTful接口呢？

1
sails generate model foo
这样我们又在api/models中得到了一个Foo.js，像下面这样：

01
/**
02
* Foo.js
03
*
04
* @description :: TODO: You might write a short summary of how this model works and what it represents here.
05
* @docs        :: http://sailsjs.org/#!documentation/models
06
*/
07
 
08
module.exports = {
09
 
10
  attributes: {
11
      nickname: 'string'
12
  }
13
};
和Mongoose之类的很像，就是定义模型了，没什么稀罕的。我们重启一下服务器，访问http://localhost:1337/foo，发现返回一个空的Array呃，怎么回事？这其实就是Foo类的所有对象哈，不过我们之前还没有创建任何对象呢。直接拿POSTMAN照着这个URL发一个nickname: ‘Kitty’的POST请求，再访问这个URL看看，有了：[{nickname: ‘Kitty’}]。

注意，我们没有写任何代码噢几乎，这一切都是Sails.js一手搞定的，能干。神奇的还不止于此——Sails.js几乎打通了普通HTTP请求和Websocket请求，也就是说，你写的所有的controller返回的字符串或者JSON结果，不加任何一行代码，用Websocket来取，也能取得到，颇得express.io的精髓啊，而且针对浏览器端，Sails.js还ship了一个sails.io.js（文档推荐用这个，v0.9版本的用户要注意了），这个文件在assets/js/dependencies下（可能有所不同），把Websocket操作封装成了AJAX的形式，自动包含在默认的视图中。打开主页，在调试console中输入（v0.9用户使用socket而非io.socket）：

1
io.socket.get('/foo/hello', function (res) { console.log(res); })
不出意外的话，就会输出 world 。

这点小伎俩可能真还没什么用，毕竟它和AJAX比也没多大优势，它能做的AJAX照样一行语句搞定了（jQuery的话）。继续看！

我们在controller中加一个订阅方法：

1
module.exports = {
2
  hello: function (req, res, next) {
3
    res.end('world');
4
  },
5
  subscribe: function (req) {
6
    Foo.watch(req.socket);
7
    // For earlier versions, use Foo.subscribe(req.socket) instead.
8
  }
9
};
我们在客户端这边写一个调用加一个listener:

1
io.socket.get('/foo/subscribe');
2
io.socket.on('foo', function (msg) { console.log(msg); })
然后像上面那样向http://localhost:1337/foo post一个包，让它再创建一个Foo对象，就在“磁盘数据库”（config/models.js中默认的，调试用）中。

就在它创建Foo对象的同时，浏览器这边就收到了一条消息：

Sails.js Websocket listener

类似地，如果我们向http://localhost:1337/foo这个URL以PUT、DELETE方式发送包，执行的操作则分别会是更新和删除，而verb也会是相应的。

实际上，默认情况下，Sails.js会在有对应model的controller中自动添加CRUD方法，在相应操作发生的时候，它会自动向订阅了相应model的socket发送通知消息。

 

说了这么多，可能你会觉得，Sails.js为我做了这么多，我会不会变成一个傻瓜呢？也就是，我怎么操控这些呢？比如，我如何才能限制model的创建操作，让登录的用户才能有权限创建一个model对象？这个其实就需要在config/routes.js下对’POST /foo’进行中间件审核了。这点在文档中没有体现，所以新鲜玩意儿有时候遇到问题了还是足够折磨人的。

就说这么多啦，总之，好好玩耍~ :)