利用 Sails.js + MongoDB 开发博客系统（3）-- 账户模块
May 3, 2015
CONTENTS
章节概述
设计
user document 设计
生成 user api
配置 MongoDB 数据库连接
安装 Sails 对 MongoDB 的依赖
配置 MongoDB 连接
模型层的基本配置
利用 Passport.js 来管理我们的账户认证
安装依赖
配置 user 实体
创建路由及视图
创建对应的视图
撰写登录验证逻辑，感谢 Giancarlo Soverini 提供教程
访问控制（ACL）
Policies
优化
表单验证
章节预告
章节概述
在本章中，你讲学习到如下知识：

Sails 中如何配置 MongoDB。
Sails 中的模型层（Models）知识，如模型属性（attributes），模型生命期回调（lifecycle callbacks）等。
利用 Passport.js 来管理我们的账户认证。
认识密码加密策略 bcrypt。
认识 Sails 的核心模块 policies。
如何在 Sails 中撰写自定义配置。
如何扩展 swig 模板引擎。
semantic-ui 中表单验证模块的应用。
设计
user document 设计
名称	说明	类型	限制
siteName	站点名称	string	必选，1~10 个字符
siteDesc	站点简介	string	可选，不超过 20 字符
email	用户邮箱	string(email)	必选，唯一
password	密码	string	必选，原始长度不少于 6 个字符
生成 user api
sails generate api user
可以看到，Sails 为我们生成了 user 相应地模型及控制器



配置 MongoDB 数据库连接
安装 Sails 对 MongoDB 的依赖
npm install sails-mongo --save
配置 MongoDB 连接
修改 config/connections.js:

module.exports.connections = {
  mongo: {
    adapter: 'sails-mongo',
    host: 'localhost', // defaults to `localhost` if omitted
    port: 27017, // defaults to 27017 if omitted
    database: 'blog' // or omit if not relevant
  }
};
模型层的基本配置
修改 config/models.js，配置模型的连接数据库位 MongoDB，并为每个模型添加 updatedAt 以及 createdAt 属性:

module.exports.models = {
  'connection' : 'mongo',
  autoCreatedAt: true,
  autoUpdatedAt: true
};
利用 Passport.js 来管理我们的账户认证
Passport.js 是 Node.js 中一个专注于登录验证的中间件，配置灵活，支持很多第三方登录验证。十分感谢 这篇教程 帮助我将 Passport.js 集成到 Sails 中。

安装依赖
安装 bcrypt ，bcrypt 被认为是比一般加盐加密更好的密码加密手段，一般的加盐加密在密码较简单是仍可能被暴力破解，bcrypt 牺牲了部分性能，来换取更高的密码存储安全，参看如下两篇文章：

How To Safely Store A Password

md5/sha1+salt 和 Bcrypt

安装 bcrypt:

npm install bcrpyt --save
安装 Passport.js:

npm install passport --save
因为我们是本地验证，所以只需要 Passport 的本地验证支持

npm install passport-local --save
配置 user 实体
在此博客系统中，用户注册前需要对用户密码进行 bcrypt 加密，这将会利用到 Sails 中模型层的生命期回调 Lifecycle callbacks , 在此，我们用到的生命期为 beforeCreate，亦即模型创建前执行的回调：

api/models/User.js:

var bcrypt = require('bcrypt');
module.exports = {
  attributes: {
    // 站点名称
    siteName: {
      type: 'string',
      required: true,
      minLength:1,
      maxLength:10
    },
    // 邮箱
    email: {
      type: 'email',
      unique: true,
      required: true
    },
    // 密码
    password: {
      type: 'string',
      required: true
    },
    // 站点简介
    siteDesc: {
      type: 'string',
      defaultsTo: '暂无简介',
      maxLength:40
    },
    // 是否管理员（默认为非管理员）
    isAdmin: {
      type: 'boolean',
      defaultsTo: false
    }
  },
  // 创建（注册）用户前，对用户密码加密
  beforeCreate: function (values, cb) {
    bcrypt.genSalt(10, function(err, salt) {
      bcrypt.hash(values.password, salt, function(err, hash) {
        if(err) return cb(err);
        values.password = hash;
        // 执行用户定义回调
        cb();
      });
    });
  }
};
创建路由及视图
先创建一个封装了登录注册逻辑的控制器 AuthController.js：

sails generate controller auth
接下来在 config/routes.js 中为我们的登录注册创建相应路由：

//config/routes.js
module.exports.routes = {
  /***************************************************************************
  *                                                                          *
  * Make the view located at `views/homepage.ejs` (or `views/homepage.jade`, *
  * etc. depending on your default view engine) your home page.              *
  *                                                                          *
  * (Alternatively, remove this and add an `index.html` file in your         *
  * `assets` directory)                                                      *
  *                                                                          *
  ***************************************************************************/
  //'/': {
  //  view: 'homepage'
  //}
  '/' : {
    view :'index'
  },
  //---------------Login & Register
    // 跳转到注册页面
    'get /register': 'AuthController.toRegister',
    // 处理注册逻辑
    'post /register': 'AuthController.processRegister',
    // 跳转到登陆页
    'get /login': {
        view: 'passport/login'
    },
    // 处理登陆逻辑
    'post /login': 'AuthController.processLogin',
    // 登出逻辑
    '/logout': 'AuthController.logout'
};
创建对应的视图
相应地 css 文件内容不给出，UI 设计见仁见智。

views/passport/layout.swig：

{% extends '../partial/layout.swig' %}
{% block stylesheets -%}
<link rel="stylesheet" type="text/css" href="{{ path.style }}/passport.css"/>
{%- endblock %}
注意，给登录及注册页面声明待调用模块：

passport/login 及 passport/register

views/passport/login.swig:

{% extends 'layout.swig' %}
{% set module = 'passport/login' %}
{% block title -%}欢迎登录{%- endblock %}
{% block form_content -%}
    <form class="ui form" action="/login" method="post">
        <h1 class="ui dividing header"> 欢迎登录 < span class="tag"></h1>
        {% if err -%}
            <div class="ui error message" style="display: block">
                <div class="header"> 登录失败 < span class="tag"></div>
                <p> 账号或密码错误 < span class="tag"></p>
            </div>
        {%- endif %}
        <div class="field">
            <input id="email" name="email" type="text" placeholder="您的邮箱">
        </div>
        <div class="field">
            <input id="password" name="password" type="password" placeholder="您的密码"/>
        </div>
        <div class="field">
            <div class="ui buttons">
                <button class="ui positive button" type="submit"> 登录 < span class="tag"></button>
                <div class="or"></div>
                <a class="ui negative button" href="/register"> 注册 < span class="tag"></a>
            </div>
        </div>
    </form>
{%- endblock %}
views/passport/register.swig：

{% extends 'layout.swig' -%}
{% set module='passport/register' %}
{% block title -%}注册{%- endblock %}
{% block form_content -%}
    <form class="ui form" action="/register" method="post">
        <h1 class="ui dividing header"> 欢迎注册 < span class="tag"></h1>
        {% if err -%}
            <div class="ui error message" style="display: block">
                <div class="header"> 注册失败 < span class="tag"></div>
                <p> 邮箱已被注册 < span class="tag"></p>
            </div>
        {%- endif %}
        <div class = "field">
            <input id="siteName" name="siteName" type="text" placeholder="填写站点名称"/>
        </div>
        <div class="field">
            <input id="email" name="email" type="email" placeholder="填写邮箱">
        </div>
        <div class="field">
            <input id="password" name="password" type="password" placeholder="填写密码（不少于 6 个字符）"/>
        </div>
        <div class="field">
            <textarea id="siteDesc" name="siteDesc" placeholder="填写站点简介"></textarea>
        </div>
        <div class="field">
            <div class="ui buttons">
                <button class="ui positive button" type="submit"> 注册 < span class="tag"></button>
                <div class="or"></div>
                <a class="ui negative button" href="/login"> 登录 < span class="tag"></a>
            </div>
        </div>
    </form>
{%-endblock %}
撰写登录验证逻辑，感谢 Giancarlo Soverini 提供教程
在 api/controllers/AuthController.js 添加如下内容：

/**
* 验证逻辑控制器
* */
var passport = require('passport');
module.exports = {
    /**
     * 处理注册逻辑
     * @param req
     * @param res
     */
    processRegister: function(req,res){
        // 由请求参数构造待创建 User 对象
        var user = req.allParams();
        User.create(user).exec(function createCB(err, created){
            if(err){
               // 如果有误，返回错误
                res.view('passport/register',{err:err});
            }else{
                // 否则，将新创建的用户登录
                req.login(created, function(err) {
                    if (err) { return next(err); }
                    return res.redirect('/');
                });
            }
        });
    },
    /**
     * 处理登陆逻辑
     * @param req
     * @param res
     */
    processLogin: function(req,res){
        // 使用本地验证策略对登录进行验证
        passport.authenticate('local', function(err, user, info) {
            if ((err) || (!user)) {
                return res.send({
                    message: info.message,
                    user: user
                });
            }
            req.logIn(user, function(err) {
                if (err) res.send(err);
                return res.send({
                    message: info.message,
                    user: user
                });
            });
        })(req, res);
    },
    /**
     * 处理登出逻辑
     * @param req
     * @param res
     */
    logout: function(req, res) {
        req.logout();
        res.redirect('/');
    }
};
任何 Sails 的控制器方法都有两个参数， req（请求）对象及 res（响应）对象。

创建 config/passport.js 对 Passport 验证进行如下配置：

var passport = require('passport'),
    // 使用本地登录逻辑
    LocalStrategy = require('passport-local').Strategy,
    // 使用 bcrypt 进行密码加密
    bcrypt = require('bcrypt');
passport.serializeUser(function(user, done) {
    done(null, user.id);
});
passport.deserializeUser(function(id, done) {
    User.findOne({ id: id } , function (err, user) {
        done(err, user);
    });
});
passport.use(new LocalStrategy({
        usernameField: 'email',
        passwordField: 'password'
    },
    function(email, password, done) {
        User.findOne({ email: email }, function (err, user) {
            if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect email.' });
            }
            bcrypt.compare(password, user.password, function (err, res) {
                if (!res)
                    return done(null, false, {
                        message: 'Invalid Password'
                    });
                var returnUser = {
                    email: user.email,
                    createdAt: user.createdAt,
                    id: user.id
                };
                return done(null, returnUser, {
                    message: 'Logged In Successfully'
                });
            });
        });
    }
));
访问控制（ACL）
将 Sails 启动后，访问 localhost:1337/register 进行用户注册:



然后可以在 MongoDB 中看到，我们成功创建了用户:



Policies
如何对系统中的页面进行访问控制，这里就要用到 Sails 的核心组件 — Policies ，接下来我们创建几个 policy 来对我们的业务逻辑进行访问控制（access control）。

首先，创建 api/policies/isAuthenticated.js，该 policy 用于判断请求是否得到授权（即用户是否登录）：

/**
 * 用户是否被授权
 * @param req
 * @param res
 * @param next
 * @returns {*}
 */
module.exports = function(req, res, next) {
    if (req.isAuthenticated()) {
        return next();
    }
    else{
        return res.redirect('/login');
    }
};
因为本博客系统为私人博客，故在跳转到注册的业务逻辑时，我们需要知道用户是否被创建，如果用户被创建，则跳转回首页，否则继续执行注册相应逻辑：

创建 api/policies/userNotCreated.js：

module.exports = function (req, res, next) {
    // 检查数据库中是否已经有用户
    User.find().exec(function(err,users){
        if(users.length){
            res.redirect('/logout');
        }else {
            next();
        }
    });
};
同时，本系统显然只有当用户存在时访问才是有效地（否则没有文章来源）。创建 api/policies/userCreated.js：

module.exports = function (req, res, next) {
    // 检查数据库中是否已经有用户
    User.find().exec(function(err,users){
        if(users.length){
            next();
        }else {
            res.redirect('/register');
        }
    });
};
然后设置 config/policies.js，建立 policy 与各业务逻辑的映射关系：

/**
 * Policy Mappings
 */
module.exports.policies = {
    // 默认所有行为需要登录
    // 若某些行为不需要，则在下面声明
    '*': 'isAuthenticated',
    // 验证逻辑都不需要登录
    // 用户创建后不再允许注册
    AuthController: {
        '*': true,
        toRegister: 'userNotCreated'
    },
};
true 代表该业务逻辑可被任何角色访问，而通配符 * 则代表所有业务逻辑。

接下来重启 sails，并访问 localhost:1337 ，我们将会被跳转登录页：



优化
诸如站点名称（siteName），站点简介（siteDesc）等属性将经常被不止一个页面所访问，如果每次我们进行数据库查询获取这两个数据将会是十分低效的，在这里，我们通过 Sails 来设置这两个属性。

创建 config/site.js：

module.exports.site = {
    // 站点名称
    name  : "",
    // 站点介绍
    desc  : "",
};
现在，在 swig 中，我们可以通过如下方式访问到这两个变量了：

{{ sails.config.site.name }}
{{ sails.config.site.desc }}
这样的书写方式太过冗长，为此，我们修改我们的视图配置，让页面维护一个 site 对象：

config/views.js:

var extras = require('swig-extras');
module.exports.views = {
    engine: {
        /* Template File Extension */
        ext: 'swig',
        /* Function to handle render request */
        fn: function (path, data, cb) {
            /* Swig Renderer */
            var swig = require('swig');
            // 保证我们在开发环境下每次更改 swig 不用重启 sails
            if (data.settings.env === 'development') {
                swig.setDefaults({cache: false});
            }
            // 维护一个 site 变量
            data.site = sails.config.site;
            // 提供一个变量标示用户是否登录
            if (typeof (data.isLogged) == 'undefined') {
                data.isLogged = !!data.req.isAuthenticated();
            }
            /*
             * 绑定一些常用路径
             * Thanks to: https://github.com/mahdaen/sails-views-swig
             * */
            var paths = {
                script: '/js',
                style: '/styles/default',
                image: '/images',
                font: '/fonts',
                icon: '/icons',
                bower: '/bower_components'
            };
            if (!data.path) {
                data.path = paths;
            }
            else {
                for (var key in paths) {
                    if (!key in data.path) {
                        data.path[key] = paths[key];
                    }
                }
            }
            // 补充 extra
            extras.useFilter(swig, 'split');
            /* Render Templates */
            return swig.renderFile(path, data, cb);
        }
    },
    layout: 'layout',
    partials: false
};
显然，每次服务器启动的时候就应当设置这两个变量，为此，我们修改 config/bootstrap.js，该文件可以配置服务器启动时的相应动作：

module.exports.bootstrap = function (cb) {
    // 启动时刷新站点信息
    User.find().exec(function(err,users){
        if(users.length > 0){
            var user = users[0];
            sails.config.site.name = user.siteName;
            sails.config.site.desc = user.siteDesc;
        }
        cb();
    });
};
而每次用户信息创建或更新时也应当更新站点配置，为此，我们在 user 模型中添加新的生命期回调： afterUpdate 及 afterCreate:

afterCreate: function (createdUser, cb) {
    this.updateSite(user);
    cb();
},
// 用户信息更新时，更新站点信息
afterUpdate: function (user,cb) {
    this.updateSite(user);
    cb();
},
// 更新站点信息
updateSite: function(user){
    sails.config.site.name = user.siteName;
    sails.config.site.desc = user.siteDesc;
}
表单验证
后端的属性验证已经在对应的 user 模型中设置好了，现在我们利用 semantic-ui 的 表单验证模块 来设置前端表单验证，让登录注册页的交互更加完整。

首先，我们要在 assets/js/common/main.js 中手动声明 semantic 表单验证组件的位置：

// 第三方模块声明
require.config({
    baseUrl: '/bower_components/',
    paths: {
        jquery: 'jquery/dist/jquery',
        requirejs: 'requirejs/require',
        'semantic-ui': 'semantic-ui/dist/semantic',
        underscore: 'underscore/underscore',
        backbone: 'backbone/backbone',
        'semantic-form': 'semantic-ui/dist/components/form.min'
    },
    packages: [
    ]
});
// 加载 app，并运行
require(['/js/common/app.js'],function(app){
    app.init();
});
接下来，我们在 assets/js/passport 目录下创建三个文件：

login.js
register.js
PassportPanel.js
其中，PassportPanel.js 中提供一个账户框视图组件，该组件继承自 Backbone.View ，供 login 及 register 两个 module 调用。

PassportPanel.js：

define(['semantic-form'], function () {
    var PassportPanel = Backbone.View.extend({
        el: $('.passportContainer'),
        events: {
            'click input': 'hideError'
        },
        initialize: function (which) {
            // 初始化时绑定表单验证
            if (which === 'login')
                this.bindLoginForm();
            else
                this.bindRegForm();
        },
        /**
         * 聚焦输入框时，隐藏错误提示
         */
        hideError: function () {
            if ($('.error').length > 0) {
                if (!$('.error').is(':hidden')) {
                    $('.error').hide();
                }
            }
        },
        /**
         * 绑定登录表单验证
         */
        bindLoginForm: function () {
            $('.ui.form').form({
                inline: true,
                fields: {
                    email: {
                        identifier: 'email',
                        rules: [
                            {
                                type: 'email',
                                prompt: '请填写正确的邮箱'
                            }
                        ]
                    },
                    password: {
                        identifier: 'password',
                        rules: [
                            {
                                type: 'empty',
                                prompt: '请填写密码'
                            }
                        ]
                    }
                }
            });
        },
        // 绑定注册表单验证
        bindRegForm: function () {
            $('.ui.form').form({
                inline: true,
                fields: {
                    siteName: {
                        identifier: 'siteName',
                        rules: [
                            {
                                type: 'empty',
                                prompt: '站点名不能为空'
                            },
                            {
                                type: 'maxLength[10]',
                                prompt: '站点名不能超过 10 个字符'
                            }
                        ]
                    },
                    email: {
                        identifier: 'email',
                        rules: [
                            {
                                type: 'email',
                                prompt: '请填写正确的邮箱'
                            }
                        ]
                    },
                    password: {
                        identifier: 'password',
                        rules: [
                            {
                                type: 'length[6]',
                                prompt: '密码不能少于 6 位'
                            }
                        ]
                    },
                    siteDesc: {
                        identifier: 'siteDesc',
                        rules: [
                            {
                                type: 'maxLength[20]',
                                prompt: '站点简介不超过 20 字符'
                            }
                        ]
                    }
                }
            });
        }
    });
    return PassportPanel;
});
login.js：

define(['./PassportPanel.js'],function(PassportPanel){
    return {
        run: function(){
            // 如果有错误，则当 focus 输入域时，自动隐藏错误提示
            var loginPanel = new PassportPanel('login');
        }
    }
});
register.js：

define(['./PassportPanel.js'],function(PassportPanel){
    return {
        run : function(){
            var regPanel = new PassportPanel('reg');
        }
    }
});
测试一下：



章节预告
在下一章当中，我们开始实现博客系统的文章模块。