---
title: Visual Studio Code
created: '2019-08-16T08:38:43.120Z'
modified: '2019-09-20T07:37:48.473Z'
---

# Visual Studio Code

Visual Studio Code 的目标是做一个 Lightweight Editor，通过的扩展 api，让用户在 VS Code 中达到和 IDE 中接近的开发体验（效率）。

作者：rebornix
链接：https://www.zhihu.com/question/36292298/answer/160028010
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

“VS Code 师出 VS，是 VS 找了一群人来重写的，复用了很多 VS 的代码，等等“。很抱歉，并不是这样，半毛钱关系也没有。VS Code 的核心代码，也就是 Microsoft/monaco-editor 是 Erich Gamma 2011 年加入微软后，招聘的一支“全新”的队伍进行开发的。Monaco editor 从一开始就是一个 browser based editor，早期一直服务于各个微软系统中（比如 Visual Studio Online，OneDrive online）。招聘的这支队伍对于 Erich 来说并不是新的，因为大部分成员都是原先 IBM 的老部下，其中几位大爷跟着 Erich 撸了二十多年代码了。"VS Code 是 Atom 的复刻，是对 Atom 的魔改，是 Atom 的一个主题！"。很抱歉，并不是这样，但还是有几毛钱关系的。Monaco Editor 在经历几年的高光期，进入了一个小小的黑暗时代。这时候团队成员开始调研将 Monaco Editor 做成桌面应用，和 Atom 一样，我们首先关注到的就是 node-webkit。必须说 node-webkit 是业界的一缕清风，给这个产业带来了太多的可能性。当然最后我们选用了 atom-shell，也就是后来的 Electron。但就是这个 atom-shell，给大家带来了以上的误导。

在细分市场中，插件生态系统对代码编辑器来说是至关重要的。微软很快就意识到了这一点，所以，它不仅自己发布了很多插件，还让第三方插件开发变得很容易。因为 VS Code 是使用 JavaScript 开发的，并且是基于 Electron 框架的，所以开发 VS Code 插件非常容易。所以，VS Code 拥有很多高质量的插件。

https://microsoft.github.io/monaco-editor/index.html

