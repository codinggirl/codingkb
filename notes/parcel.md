

// Parcel 是一个前端项目打包工具。

# Parcel

Parcel 是一个网络应用打包工具。和 webpack 有着同样的目的，但有着不同的价值观。

## 主要功能

// 参阅：[Parcel, a simpler webpack][Flavio Copes: Parcel, a simpler webpack]
- 资产打包。打包 js、css、html、图片等。
- 无配置代码分割
- 自动转换。通过 Babel、PostCSS、PostCSS。
- 模块热替换
- 缓存、并行处理，更快的构建速度

## Parcel 网站

// Parcel 的网站是很恶心的。从 Google 浏览器中搜到一个内容，一看是英文的 Parcel 内容，资料正好是你所需要的。然而点击去后，自动跳转到了中文首页。
// 我们无法理解这种做法。

我们建议直接访问英文版网站：[Parcel][Parcel 英文版]

[Parcel 英文版]: https://en.parceljs.org/

### Parcel让人眼前一亮

在用了很久Webpack后用Parcel的感觉就像用了很久Android机后用iPhone，不用再去操心细节和配置，大多数时候Parcel刚刚够用而且用的很舒服。

用Parcel去完成以上项目的要求，我只是专心去写项目页面所必须的代码，Parcel智能快速的帮我构建出了能正常运行的结果。

以下是Parcel让我心动的点：

*   Parcel能做到无配置完成以上项目构建要求；
*   Parcel内置了常见场景的构建方案及其依赖，无需再安装各种依赖；
*   Parcel能以HTML为入口，自动检测和打包依赖资源；
*   Parcel默认支持模块热替换，真正的开箱即用；

而反观Webpack，比Parcel要麻烦很多：

*   需要写[一堆配置](https://github.com/gwuhaolin/parcel-vs-webpack/blob/master/webpack.config.js)；
*   需要再安装[一堆依赖](https://github.com/gwuhaolin/parcel-vs-webpack/blob/master/package.json)；
*   不能简单的自动生成HTML；

这个项目我用Parcel时花在构建配置上的时间不到一分钟，而用Webpack构建时花了5分钟去配置。

### Parcel还需要时间去打磨

通过以上项目实践，发现Parcel目前有如下明显的缺点：

*   **不支持SourceMap**：在开发模式下，Parcel也不会输出SourceMap，目前只能去调试可读性极低的代码；
*   **不支持剔除无效代码(TreeShaking)**：很多时候我们只用到了库中的一个函数，结果Parcel把整个库都打包了进来；
*   **一些依赖会让Parcel出错**：当你的项目依赖了一些Npm上的模块时，有些Npm模块会让Parcel运行错误；

### Parcel需要为零配置付出代价

零配置其实是把各种常见的场景做为默认值来实现的，这虽然能节省很多工作量，快速上手，但这同时会带来一些问题：

*   **不守规矩的node\_module**：有些依赖的库在发布到Npm上时可能不小心把`.babelrc` `postcss.config.js` `tsconfig.json`这些配置文件也一起发布上去了， 由于目前Parcel只要在目录中发现这些配置文件就会认为该项目中的代码需要被处理。例如mini\-store这个库中就把 `.babelrc`文件发布到了Npm上，项目依赖的本来是lib中已经编译成了ES5的JS代码了，但Parcel还会去用Babel处理一遍。 Npm官方并没有规定发布到Npm上的包需要符合哪些规范，这会让Parcel很为难。
*   **不灵活的配置**：零配置的Parcel关闭了很多配置项，在一些需要的配置的场景下无法改变。例如：
    *   无法控制对部分文件的特殊处理，以实现诸如按需加载这样的需求；
    *   无法控制[输出文件名的Hash值和名称](http://webpack.wuhaolin.cn/2%E9%85%8D%E7%BD%AE/2-2Output.html)；
    *   无法控制构建输出目录结构；
    *   无法[映射路径以缩短导入语句](http://webpack.wuhaolin.cn/2%E9%85%8D%E7%BD%AE/2-4Resolve.html)；
    *   HTTP开发服务器不支持[HistoryApi](http://webpack.wuhaolin.cn/2%E9%85%8D%E7%BD%AE/2-6DevServer.html)；

### Parcel使用场景受限

目前Parcel**只能用来构建用于运行在浏览器中的网页**，这也是他的出发点和专注点。 在软件行业不可能存在即使用简单又可以适应各种场景的方案，就算所谓的人工智能也许能解决这个问题，但人工智能不能保证100%的正确性。

反观Webpack除了用于构建网页，还可以做：

*   [打包发布到Npm上的库](http://webpack.wuhaolin.cn/3%E5%AE%9E%E6%88%98/3-13%E6%9E%84%E5%BB%BANpm%E6%A8%A1%E5%9D%97.html)
*   [构建Node.js应用(同构应用)](http://webpack.wuhaolin.cn/3%E5%AE%9E%E6%88%98/3-11%E6%9E%84%E5%BB%BA%E5%90%8C%E6%9E%84%E5%BA%94%E7%94%A8.html)
*   [构建Electron应用](http://webpack.wuhaolin.cn/3%E5%AE%9E%E6%88%98/3-12%E6%9E%84%E5%BB%BAElectron%E5%BA%94%E7%94%A8.html)
*   [构建离线应用(ServiceWorkers)](http://webpack.wuhaolin.cn/3%E5%AE%9E%E6%88%98/3-14%E6%9E%84%E5%BB%BA%E7%A6%BB%E7%BA%BF%E5%BA%94%E7%94%A8.html)

### 构建速度和输出文件大小对比

分别去用Parcel和Webpack构建以上项目，收集的数据如下：

| 数据项 | Parcel | Webpack |
| --- | --- | --- |
| 生成环境构建时间 | 8.310s | 9.58s |
| 开发环境启动时间 | 5.42s | 8.06s |
| 监听变化构建时间 | 3.17s | 2.87s |
| 生成环境输出JS文件大小 | 544K | 274K |
| 生成环境输出CSS文件大小 | 23K | 23K |

从以上数据可以看出：**Parcel构建速度快，但Parcel输出文件大**

导致Parcel构建速度快的原因和iOS比Android用起来更流畅的原因类似：

*   Parcel因为一体化内置，所以集成和优化的更好，而Webpack通过插件和Loader机制去让第三方扩展这会拉低性能；
*   Parcel内置多进程并行构建，而Webpack默认是单进程构建（[Webpack也支持多进程](http://webpack.wuhaolin.cn/4%E4%BC%98%E5%8C%96/4-3%E4%BD%BF%E7%94%A8HappyPack.html)）；

导致Parcel输出JS文件大的原因在于：

*   不支持TreeShaking
*   构建出的JS中出现了所有文件的名称，如图： ![](https://user-gold-cdn.xitu.io/2017/12/27/1609828804675828?w=2206&h=546&f=png&s=307152)

> 以上[项目完整源码可下载](https://github.com/gwuhaolin/parcel-vs-webpack)

## 生产环境打包

- 禁止模块热替换
- 不再监视文件变动
- 使文件精简化
- 触发环境变量，`NODE_ENV` 为 `production`

## 如何选择

- Parcel 用于小项目
- webpack 用于大型、不断增长的项目，和你想掌控一切的项目

## 参考资料

[Parcel Vs Webpack - 浩麟的博客](http://wuhaolin.cn/2017/12/27/Parcel%20Vs%20Webpack/)

[Parcel, a simpler webpack][Flavio Copes: Parcel, a simpler webpack]


[Flavio Copes: Parcel, a simpler webpack]: https://flaviocopes.com/parcel/ "Parcel, a simpler webpack"
