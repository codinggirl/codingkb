---
title: Untitled (2)
created: '2019-09-18T07:51:35.106Z'
modified: '2019-09-18T07:52:10.012Z'
---

# Untitled (2)

https://michaelyou.github.io/2018/05/15/%E7%BF%BB%E8%AF%91-%E4%B8%8D%E4%BD%BF%E7%94%A8%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93/

[原文](https://blog.gopheracademy.com/advent-2014/case-against-3pl/)

译者：不少golang开源项目的Contrib Guidelines里会表示在提交PR时尽量不要引入第三方的包，这篇文章正是阐述了这样一个观点。原文标题下还有一行是：”我如何不再担心包管理并开始喜欢上无版本的包管理”，可以作为本文的另一个标题。也许很多人并不认同本文的观点，甚至认为是胡说八道，但是确实有很多项目在使用这样一个规则：`influxdb`的[coding guidlines](https://github.com/influxdata/influxdb/blob/master/CODING_GUIDELINES.md#try-not-to-use-third-party-libraries)明确表示*Try not to use third\-party libraries*，web框架[gin](https://github.com/gin-gonic/gin)的router实现copy了[github.com/julienschmidt/httprouter](https://github.com/julienschmidt/httprouter)的代码实现并做了少量更改，几乎所有的大型golang程序都会自己实现一些通用的函数，例如slice的union、unique等操作，自己扩展log的程序也有很多，比如nsq。

---

如果你看过[golang\-nuts](https://groups.google.com/forum/#!forum/golang-nuts)邮件列表，会发现包管理比任何其他话题都容易引起争论。在我刚开始写Go的时候同样认为缺少一个`真正的`包管理是一个明显的失误，但是，当我写的Go代码越多我越是开始欣赏`go get`的简单易用。

版本化软件包的好处很明显。它使团队中的每个人都有一份一致的代码，并且大多数时候包的版本号采用固定的模式([语义化版本](https://semver.org/))，可以传达API的变更信息。但是包版本控制并非没有缺点。

在这篇文章中，我们将来探讨应用程序开发人员可用的选项，库开发人员的职责以及无版本包管理的副作用。

### [](#只使用标准库 "只使用标准库")只使用标准库

许多第三方Go库试图提供较现有的标准库Package更丰富的实现。然而，很多时候，他们的功能集变得太大而无法正确测试，并且需要新的开发人员了解额外的框架，从而增加了项目的认知开销。

在GitHub上有[200多个Go Web框架](https://github.com/search?l=Go&p=3&q=web+framework&type=Repositories)，我一个都没用过。基于HTTP的项目有这样一系列的要求和权衡，我很欣赏`net/http`提供的功能和实用性的平衡。如果我的应用程序需要中间件，那么写一个适合我的项目的`http.Handler`是很简单的。没有万能的Web框架。

日志记录(Logging)是另一个`少即是多(less is more)`的领域。 GitHub上有[超过500多个Go日志库](https://github.com/search?l=Go&p=3&q=logging&type=Repositories)，但我更喜欢标准日志包(**log**)的简单。它提供了一个小型的、固定范围的库，将时间戳信息打印到标准错误。这就是我需要的。添加其他功能，如日志等级，让开发人员选择何时使用DEBUG而不是INFO级别，会增加剩余代码的复杂性。如果足够重要，那就应该记录日志。

### [](#Ctrl-C-Ctrl-V "Ctrl-C, Ctrl-V")Ctrl\-C, Ctrl\-V

[DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的坚定支持者可能会对此建议感到畏缩，但多次将小部分代码复制到项目中可能是导入整个库的更好选择。 如果需要，将小功能复制过来使你可以根据自己的特定需求对其进行调整，并随时扩展它们。 它们还限制了项目中其他开发人员所需的知识，因为他们不需要读取另一个库的完整文档来了解依赖关系。

几个月前，我发布了一个没有`.go`文件的Go[测试库](https://github.com/benbjohnson/testing)，因为它足够小，我希望用户只需复制他们需要的功能。 另一个很好的例子是使用一些简单的算法，如[一致的哈希](https://github.com/benbjohnson/jmphash)。 很多时候他们少于50行代码。 在复制任何代码之前，请查看包的证书。

### [](#固定范围的库 "固定范围的库")固定范围的库

尽管我试图坚持使用标准库，但有时候我还是需要使用第三方库。因为`go get`不提供版本控制，所以与使用像[rubygems](http://rubygems.org/)等工具时相比，我对第三方库的看法非常不同。

在Ruby中，在引用程序中肆意添加第三方库很常见。一个三年的Rails项目很容易在其`Gemfile`中拥有50个依赖项。随着时间的推移，我需要升级库，毫无疑问，它们的功能或API会改变，并与我使用的其他库冲突。所以我将不得不升级那些可能导致更多冲突的其他库。

那时我意识到一个有30个版本的库实际上是30个独立的库。

在Go中，我尝试编写和使用具有固定范围的库，因此他们只需要一个版本。例如，[css](https://github.com/benbjohnson/css)库实现了[W3C的CSS语法模块级别3](http://www.w3.org/TR/css3-syntax/)规范，因此其范围是固定的。一旦实施和测试，变更和bug修复都会很小。

另一个例子是[Bolt](https://github.com/boltdb/bolt)库。它的目标是提供一个简单，快速的事务密钥/值存储。通过限制它的问题空间，Bolt能够保持最小的API并经过良好测试。经过近六个月的各种生产系统运行没有问题，Bolt升级到了1.0版并停止了开发。该项目不会被放弃 \- 它只是已经完成了。添加功能会影响项目的稳定性。 Bolt的下一个版本不会是2.0，它会被简单地称为别的东西。

### [](#高质量建立信任-amp-信任建立社区 "高质量建立信任&信任建立社区")高质量建立信任&信任建立社区

无版本软件包管理的一个意想不到的副作用是，我现在需要知道并信任编写我使用的库的开发人员。 正因为如此，我在Go社区的参与度比我在任何其他语言社区的参与度都要高。

我定期阅读我使用的项目源代码，看看Package维护人员的风格和项目质量是否与我期望的一致。它让我对社区中的开发者更加赞赏。了解某人的代码是非常个人化的事，我相信这是Go社区蓬勃发展的一个小原因。

### [](#结论 "结论")结论

Go语言提供了惊人的高质量和深思熟虑的通用库的实现。很多时候，这是项目所需的一切。 很多时候你也会需要超出标准库范围的功能。在这些情况下，我希望你能花时间了解一下你使用的库的代码和开发人员。
