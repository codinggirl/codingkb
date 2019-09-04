---
tags: [GitHub]
title: cross-env使用以及根据环境打包
created: '2019-09-04T09:10:40.951Z'
modified: '2019-09-04T09:10:44.270Z'
---

## cross\-env使用以及根据环境打包

发布于 2019年6月17日 / [学习](https://www.ahwgs.cn/category/study)

### 关于

*   之前的项目打包都是靠手动去改环境变量(纯属沙雕行为)，随着项目越来越多，每一个项目打包都要去改这个变量的话真的是太蛋疼了，所以研究了一下`webpack`打包以及`node env`

### 主要

*   这样做有什么好处?

```javascript
  publicPath: process.env.APP_ENV === 'production' ? 'https://cdn.xxxx.com/brand-mall-chengdong/' : '/',
  outputPath: './brand-mall-chengdong',

```

JavaScript

之前都是每次打包手动修改这个静态资源的地址，修改之后根据环境变量自动区分

*   第一步，安装`cross-env`

```bash
yarn add cross-env@5.1.1 cross-port-killer@1.0.1

```

Bash

```
什么是cross-env?
解:当您使用NODE_ENV=production类似设置环境变量时，大多数Windows命令提示将会阻塞 。（例外是Windows上的Bash，它使用本机Bash。）同样，Windows和POSIX命令如何利用环境变量也有所不同。使用POSIX，您可以使用：$ENV_VAR 和您使用的Windows %ENV_VAR%。

```

– 第二步，修改`package.json`文件

```
    "build": "cross-env APP_ENV=production umi build",
    "build:test": "cross-env APP_ENV=test umi build",

```

新增一条如上命令,当执行`npm run build`时，设置`proess.env.APP_ENV`为`production`,同理设置为`test`.然后在`config.js`文件中即可根据这个变量设置相应的路径。

### 关于

*   文章首发于[cross\-env使用以及根据环境打包](https://www.ahwgs.cn/cross-env-build.html)

[\# node](https://www.ahwgs.cn/tag/node)

本文采用 [CC BY\-NC\-SA 3.0 Unported](https://creativecommons.org/licenses/by-nc-sa/3.0/deed.zh) 协议进行许可
本文链接： [https://www.ahwgs.cn/cross\-env\-build.html](https://www.ahwgs.cn/cross-env-build.html "cross-env使用以及根据环境打包")

