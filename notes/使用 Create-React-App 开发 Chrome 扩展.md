---
tags: [Chrome 扩展, React]
title: 使用 Create-React-App 开发 Chrome 扩展
created: '2019-09-04T06:21:15.295Z'
modified: '2019-09-04T06:21:32.610Z'
---

# 使用 Create\-React\-App 开发 Chrome 扩展

2019\-07\-07 22:28

[“技术”](https://nusr.github.io/categories/%E6%8A%80%E6%9C%AF/)

约 1318 字 预计阅读 3 分钟 87 次阅读

## 文章目录

*   [初始化项目](#初始化项目)
*   [支持 Sass](#支持-sass)
*   [读取文件](#读取文件)
*   [解析文本](#解析文本)
*   [配置 manifest.json](#配置-manifest-json)
*   [上传到 Chrome 网上应用商店](#上传到-chrome-网上应用商店)

> 整理 Kindle 标注、书签和笔记从未如此简单！

Kindle 标注管理应用 [Kindle Mate](https://kmate.me/cn/) 只支持 Windows，不支持 Mac。标注只是解析**我的剪贴**文本文件，配合 [FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader) API，写个 Chrome 扩展解析文本文件就好了。

*   [Chrome 下载地址](https://chrome.google.com/webstore/detail/my-clippings/pcfdnhenjhhcbfbanepfegljllniecpe)

*   [Github 地址](https://github.com/nusr/my-clippings)

## [](#初始化项目)初始化项目

安装 React 脚手架 create\-react\-app

|

```bash
1

```

 |

```bash
npm i -g create-react-app
```

 |

初始化项目

|

```bash
1

```

 |

```bash
create-react-app my-clippings --typescript
```

 |

**\-\-typescript** 表示使用 typescript。强烈推荐使用 ts，Vue 3.x 使用 ts 重写，Angular 2.x+ 只能使用 ts，ts 是大势所趋。

删除 **src/serviceWorker.ts** 文件，用不着。

## [](#支持-sass)支持 Sass

安装 node\-sass 即可。 若要支持 css modules，文件名从 **index.scss** 改为 **index.module.scss** 即可。

|

```bash
1

```

 |

```bash
npm i -D node-sass
```

 |

## [](#读取文件)读取文件

读取的文件是 Kindle 中的 **/documents/My Clippings.txt** 文件，Kindle 设置任何语言都是这个文件。只不过在不同语言下的显示不一样。 使用 [FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader) API 可以轻松读取文本文件。

|

```html
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

```

 |

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>

  <body>
    <input type="file" accept=".txt" id="file-upload" />
    <script>
      const dom = document.querySelector("#file-upload");
      dom.onchange = function(event) {
        const file = event.currentTarget.files[0];
        const reader = new FileReader();
        reader.readAsText(file);

        reader.onload = () => {
          const temp = reader.result;
          console.log(temp);
        };
      };
    </script>
  </body>
</html>
```

 |

## [](#解析文本)解析文本

要解析的文本为下面的格式，使用简单的正则表达式即可解析。 **根据 Kindle 设置的语言，中文与英文的格式会稍有不同，需要做不同的解析。**

|

```txt
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

```

 |

```txt
==========
娱乐至死 (尼尔·波兹曼)
- 您在位置 #1882-1884的标注 | 添加于 2019年6月2日星期日 上午10:07:30

古希腊哲学家在2500年以前就说过，人常常以自己的形象塑造上帝。现在，电视政治又添了新招：那些想当上帝的人把自己塑造成观众期望的形象。
==========
娱乐至死 (尼尔·波兹曼)
- 您在位置 #1925-1927的标注 | 添加于 2019年6月2日星期日 上午10:11:09

历史的消失根本不需要如此残酷的手段，表面温和的现代技术通过为民众提供一种政治形象、瞬间快乐和安慰疗法，能够同样有效地让历史销声匿迹，也许还更恒久，并且不会遭到任何反对。
==========
娱乐至死 (尼尔·波兹曼)
- 您在位置 #1961-1963的标注 | 添加于 2019年6月2日星期日 上午10:14:42

自由不是靠关掉电视实现的。电视对于大多数人来说，是生活中最有吸引力的东西。我们生活在一个绝大多数人不会关掉电视的世界里。如果我们不直接从电视得到某种信息，我们也会通过其他人得到它。
==========
```

 |

## [](#配置-manifest-json)配置 manifest.json

发布 Chrome 应用需要配置 manifest.json 。 更多设置可以查看 [开发文档](https://developer.chrome.com/extensions/manifest)

|

```json
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
25
26
27
28
29
30
31

```

 |

```json
{
  "name": "My Clippings", // 扩展名
  "short_name": "Clippings",
  "description": "Organizing your Kindle Highlight、Bookmark and Notes so easy.", // 描述
  "version": "0.0.6", // 版本号，每次提交到 chrome 应用中心
  "version_name": "0.0.6",
  "manifest_version": 2,
  "author": "Steve Xu",
  "browser_action": {
    // 点击应用图标的操作
    "default_icon": "icon-48.png",
    "default_title": "My Clippings"
  },
  "homepage_url": "https://github.com/nusr/my-clippings",
  "offline_enabled": true, // 允许离线运行
  "permissions": ["activeTab", "tabs", "storage"], // 使用的 chrome 权限
  "icons": {
    // 图标
    "16": "icon-16.png",
    "32": "icon-32.png",
    "48": "icon-48.png",
    "64": "icon-64.png",
    "128": "icon-128.png",
    "200": "icon-200.png"
  },
  "content_security_policy": "script-src 'self'; object-src 'self';", // 允许加载的文件
  "background": {
    "scripts": ["background.js"], // 外部运行的 js
    "persistent": false
  }
}
```

 |

|

```js
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
25

```

 |

```js
// background.js
// 点击扩展图标，打开新的 Tag 页面
function activeNewTab() {
  var chrome = window.chrome;
  if (!chrome) {
    return;
  }
  chrome.browserAction.onClicked.addListener(function() {
    var url = chrome.extension.getURL("index.html");
    if (window.tabId) {
      chrome.tabs.update(window.tabId, { selected: true });
    } else {
      chrome.tabs.create({ url: url }, function(tab) {
        window.tabId = tab.id;
      });
    }
  });
  chrome.tabs.onRemoved.addListener(function(tabId) {
    if (tabId === window.tabId) {
      window.tabId = null;
    }
  });
}

activeNewTab();

```

 |

## [](#上传到-chrome-网上应用商店)上传到 Chrome 网上应用商店

Chrome 网上应用商店上传地址是 [https://chrome.google.com/webstore/developer/dashboard](https://chrome.google.com/webstore/developer/dashboard)。

上传应用需要支付 **5 美元**的开发者费用。对于没有信用卡的人来说，淘宝可以找到相关代付途径，或者寻求朋友帮助。

支付完成就可以上传应用了，一个账号最多上传 **20** 个应用。

> 首发 [https://nusr.github.io/](https://nusr.github.io/)

文章作者 Steve Xu

上次更新 2019\-07\-07 22:28

许可协议 [MIT](https://github.com/nusr/blog/blob/master/LICENSE)
