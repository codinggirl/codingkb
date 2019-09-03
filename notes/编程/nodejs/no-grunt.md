---
tags: [Sails]
title: Sails中禁用Grunt
created: '2019-07-02T01:42:07.495Z'
modified: '2019-09-03T09:43:39.735Z'
---

方法一：删除或重命名 `Gruntfile.js`

方法二：配置 `.sailsrc`

```json
{
    "hooks": {
        "grunt": false
    },
    "paths": {
        "public": "assets"
    }
}
```

注意，一定要在.sailsrc配置这个，不然访问静态资源assets时返回404

```json
"paths": {
    "public": "assets"
}
```

## 参考资料

[Disabling Grunt](https://sailsjs.com/documentation/concepts/assets/disabling-grunt)

[sails.js 禁用Grunt，减少CPU开销](http://blog.csdn.net/zdb330906531/article/details/73201469?%3E)
