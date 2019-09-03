Node.js的web开发框架express、koa等，简单，小巧，精致，缺点是集成度不够，目前已有的mean或yo或sails等总有某种方面的不满意

所以我们需要做的

固化项目结构
限定orm
自定义脚手架
偏偏Node.js提供了2点，可以让你30分钟写一个脚手架

cli命令模块，编写非常容易
基于js的模板引擎（知名的30+）

MEAN是目前最潮的全栈javascript架构

MEAN是一个Javascript平台的现代Web开发框架总称，它是MongoDB + Express +AngularJS + NodeJS 四个框架的第一个字母组合。它与传统LAMP一样是一种全套开发工具的简称。

从我的角度看

mysql用mongodb替换，nosql里最像rdbms的，从开发和性能都是有优势的（老毕已经讲过了）
angular的出现是一个时代，ioc，双向绑定，指令等都曾让无数热血沸腾
nodejs提供了完全的生态和工具链，你要的它基本都有，感谢npm，早些年nodejs的性能甩php几条街的
express作为nodejs示范项目，它非常精简，是比较合适的web框架
我为什么选择MEAN架构？

成熟、稳定，简单，有问题我们能cover住，所以我们选了nodejs
把握趋势，以后nodejs的前景非常看好，尤其先后端统一，全栈方向
在架构上可以屏蔽可能风险，不孤注一掷，也不会一叶障目，合理的使用其他语言，只要每个功能都以服务出现，至于它是什么语言写的，并不重要
招人成本的性价比相对较高，技术栈新，容易吸引人才
最重要的一件事儿，是当有问题的时候，有人能cover住，在创业初期这是最最重要的事儿。

s流程控制的演进过程，分以下5部分

1) 回调函数Callbacks
2) 异步JavaScript
3) Promise/a+规范
4) 生成器Generators/ yield(es6)
5) Async/ await(es7)

目前所有版本都支持Promise/a+规范
目前Node.js 4.0 + 支持Generators/ yield
目前不支持ES7里的Async/await，但可以通过babel实现

Node.js Web开发
express、koa
restify、hapi
其他框架sails、meteor
各种类型web开发都支持的，一般我们采用非restful的使用express、koa更简单

如果是纯restful，可以采用restify、hapi

另外还有快速模拟api的json-server，对rest支持超方便

Node.js模块开发
普通模块
cli
脚手架scaffold
c/c++ addons
普通模块和cli模块只是差package.json里的

  "preferGlobal": "true",
  "bin": {
    "kp": "kp.js"
  },
脚手架scaffold = cli + 模板生成，在Node.js里这2点都非常容易

在Node.js里写c/c++扩展，有nan抽象层，其他就看大家的c/c++水平了


使用npm模块化
使用npmjs的private私有模块（目前做法）
使用npm的本地模块开发方法（测试和部署都非常快）
搭建npm私服（todo）

编写生成器
在web开发里，写了moajs生成器，类似于rails

moag order name:string password:string
其他开发，如iOS开发里模型校验非常烦,于是写了一个json2objc命令行工具，读取json，生成oc代码，可以节省不少时间

4.7.2. 2）moa-frontend
技术栈

express
jade
bootstrap、bootstrap-table
jquery
gulp
nginx
4.7.3. 3）moa-api
技术栈

base2(mirco kernel)
mongoose
bluebird
res.api
Features

自动加载路由
支持mongodb配置
集成mongoosedao，快速写crud等dao接口
自带用户管理
使用jsonwebtoken做用户鉴权
支持migrate测试
支持mocha测试
默认集成res.api，便于写接口
集成supervisor，代码变动，自动重载
gulp自动监控文件变动，跑测试
gulp routes生成路由说明
使用log4js记录日志


