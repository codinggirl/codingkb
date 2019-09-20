---
tags: [Sails]
title: file-upload
created: '2019-07-02T01:42:07.323Z'
modified: '2019-09-03T09:44:01.721Z'
---

利用 Sails.js + MongoDB 开发博客系统（5） -- 个人信息维护
May 5, 2015
CONTENTS
章节概述
设计
配置路由
业务逻辑撰写
总结
章节概述
在本章中，你将能学到如下知识:

利用 skipper 来上传和存储文件。
设计
对于个人信息维护模块，我们暂时实现如下功能：

头像更改：站点的头像仅支持 jpeg 格式，并保存到：/assets/images/avatar.jpg。
站点信息维护：包括站点名称以及站点介绍。
其他诸如密码修改等功能留给读者自行完成。

配置路由
config/routes.js 添加:

//---------------User Profile
    '/user/profile': 'UserController.index',
    '/user/profile/avatar': 'UserController.setAvatar',
业务逻辑撰写
Sails 中通过 skipper 实现了文件上传，其核心函数是 upload()，其第一个参数可接受如下配置:

dirname：上传文件保存目录
maxBytes：总的上传（不是单个文件）大小限制
saveAs：如果该参数是字符串，表示单个文件的保存名，如果是函数，则对每个上传文件进行设置
onProcess：每此上传过程中的回调逻辑
api/controllers/UserController.js：

module.exports = {
    // 个人信息修改首页
    index: function(req,res){
        if(req.method === 'GET'){
            return res.view('user/index');
        }
    },
    // 跳转到设置头像页面
    setAvatar: function (req, res) {
        if (req.method === 'GET')
            return res.view('user/avatar');
        // POST 请求进行文件上传
        // 判断是否有文件上传
        if (!req.file('avatar')._files[0]) {
            return res.badRequest('No file was uploaded');
        }
        // 判断文件类型是否争取
        var fileType = req.file('avatar')._files[0].stream.headers['content-type'];
        if (fileType != 'image/jpeg') {
            return res.badRequest('文件类型错误, 仅支持 JPG 文件格式');
        }
        req.file('avatar').upload({
            maxBytes: 10000000,
            dirname: '../../assets/images',
            saveAs: 'avatar.jpg'
        }, function whenDone(err, uploadedFiles) {
            if (err) {
                return res.negotiate(err);
            }
            return res.redirect('/');
        });
    }
};


图像预览我选用的插件是 JavaScript-Load-Image
