SailsJS连接mysql数据库
原创 2016年05月16日 22:38:55 标签：mysql /数据库 /javascript /sailsjs 1922
1.安装好必要的模块，包括sails-mysql

2.在config/connections.js添加数据库信息（所有的js代码都是用coffeescript书写，不清楚的可以在 js2coffee.thomaskalka.de/ 转成js代码阅读）


module.exports.connections = {
  'default': 'mysql'，                                        
  mysql: {
    module    : 'sails-mysql',
    host      : 'localhost',
    port      : 3306,
    user      : 'root',
    password  : 'root',
    database  : 'md5',
    charset   : 'utf8'
  }
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
3.config/models.js文件夹中配置需要使用的数据库（最新版本的sails已经不用adapter.js连接数据库了）


module.exports.models = {
  connection: 'mysql',
  migrate: 'alter'
};
1
2
3
4
5
4.在api/models/文件夹下定义数据模型，例如

module.exports = {
  tableName: 'userInfo',
  adapter: 'mysql',
  autoCreatedAt: false,
  autoUpdatedAt: false,
  attributes: {
    'username': {
      type: 'string',
      size: 50,
      unique: true,
      primaryKey: true
    },
    'password': {
      type: 'string',
      size: 30
    }
  }
};
注意数据模型的各个属性要和数据库中的一致
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
5.在api/controllers/文件夹下新建controller文件，可以对model进行操作，sailsjs支持waterline语法，例如

module.exports = {
  login: (req, res) ->
    userInfo.findOne ( {username:req.body.username} )
    .then (data) ->
      if data.password is req.body.password
        res.send "login success"
      else
        res.send "password err"
    .catch (err) ->
      res.send "no username"
};
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
这个方法中调用findone对数据库进行查询操作

其他的操作方式可以搜索waterline了解

sailsjs连接mysql就是这么多内容了