---
tags: [node]
title: npm穿墙
created: '2019-09-03T03:16:44.023Z'
modified: '2019-09-03T09:21:40.762Z'
---

# npm穿墙

原文地址：[http://www.barretlee.com/blog/2014/03/31/npm\-cross\-wall/](http://www.barretlee.com/blog/2014/03/31/npm-cross-wall/)

翻墙翻了好多年了，原理其实也挺简单的，proxy 嘛！

## 方法一：使用可用的源

国内源，[http://cnpmjs.org](http://cnpmjs.org)

使用方式，你可以在 cmd 中键入 `npm install -g cnpm`，然后出去吃个饭，如果还没有安装好，那就换个方式：

```
npm install -g cnpm --registry=http://r.cnpmjs.org

```

registry 参数的作用就是指向需要 download 的仓库。 cnpm 跟国外的 npm 是同步的，只要 npm 有更新，cnpm 就会跟着一起更新。

当然，你也可以简单点搞：

```
npm config set registry="http://r.cnpmjs.org"

```

在配置中直接指定源头，下次就没有必要使用 `--registry` 参数了。配置好了之后，npm 就指向了国内的仓库。

**B)** 当然，你也可以安装 cnpm，安装好了之后使用 cnpm 来下载文件，其实原理跟上面是一样的，于是你就可以这样了：

```
cnpm install -g package_name

```

这种方法，会导致 package-lock.json 文件中的地址改变。如果您的代码需要跨越许多城市，不建议使用。

## 方法二：代理

在配置中设置代理参数：

```
# 全局路径，也就是 npm install -g，这里 -g 的意义
npm config set prefix="c:\nodejs"

# 一般使用 goagent 翻墙，他的默认端口是 8087
npm config set proxy=http://127.0.0.1:8087

# 设置 https 的代理
npm config set https_proxy=http://127.0.0.1:8087

# 这个地方记得设置下，别搞了个代理，结果在国内源下载
npm config set registry=http://registry.npmjs.org

```

配置好代理之后，每次运行安装命令前都要确保代理服务器、代理软件正常运行。

```
npm config set proxy=http://127.0.0.1:8087

```

为啥呢，npm \-g 没必要自己去配置， registry 默认就是 `http://registry.npmjs.org`，不配置 https\_proxy，也走的通，所以就只剩下上面这条命令了。

## 方法三：直接下载到本地

直接把文件下载下来，然后放到 node\_module 之中就行了。如果是全局模块，找到全局 node\_module 的位置，然后解压放进去就行了。

这种做法，可能会造成某些模块无法使用。部分模块需要本地化编译，还有一些模块会有前置或后置操作。也有一些模块针对不同的平台有不同的文件。


