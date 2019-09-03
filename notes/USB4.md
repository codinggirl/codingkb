---
tags: [USB]
title: USB4
created: '2019-09-03T03:26:05.331Z'
modified: '2019-09-03T09:31:08.199Z'
---

# USB4

> 原文地址：[USB 4: Everything We Know So Far](https://www.tomshardware.com/news/usb-4-faq,38766.html)
> 原文时间：2019年3月7日12:45
> 翻译计划：[https://github.com/barretlee/translation\-plan](https://github.com/barretlee/translation-plan)
> 翻译人员： [小胡子哥](https://www.barretlee.com/about/)

USB Promoter 小组近期宣布新标准 “USB4”（官方拼写是木有空格的，本文会根据读者往常习惯增加一个空格，即 USB 4）将于今年年底发布，使用该技术的的产品可能会晚些时候到来。USB 4 带来了许多优势，包括更快的传输速度、更好的视频管理，以及可选的 Thunderbolt 3 接口兼容。

![upload successful](https://www.barretlee.com/blogimgs/shutterstock-by-sergei-kardashev.png)

USB 3.2 发布了四种版本，USB 3.1 发布了两种类型，还有一系列不同类型和规格的其他连接器，新标准的出现也有些势不可挡，下面我们一起来看看 USB 4 的一些特性。

### [](#USB-4-的主要优点 "USB 4 的主要优点")USB 4 的主要优点

相比之前的 USB 版本，USB 4 有下列三个重要优点：

*   **最高 40 Gbps 的传输速度**：采用双通道电缆，一些设备将可以达到与 Thunderbolt 3 相同的速度，接近 40Gbps；
*   **与 Thunderbolt 3 设备兼容**：大部分 USB 4 接口与 Thunderbolt 3 设备兼容；
*   **对视频更好的资源分配**：如果你正在使用 USB 4 接口同时传输视频和其他数据，接口将精确地分配带宽。譬如，视频只需要 20% 的带宽便可以驱动 1080p 的显示器（或集线器），那么另外 80% 的带宽将被分配给其他文件进行数据传输。

### [](#将使用-Type-C-端口 "将使用 Type-C 端口")将使用 Type\-C 端口

毫无疑问 USB 4 只能通过 Type\-C 连接器使用，不要指望看到带有老式 Type\-A 接口的 USB 4 设备或者集线器。这并不奇怪，你看，最近的苹果的 USB 供电器就是用的 Type\-C。原因很简单，如果使用 Type\-A 3.1 接口，速度和功率将会下降很多。

![upload successful](https://www.barretlee.com/blogimgs/type-c-port.png)

### [](#与-Thunderbolt-3-兼容 "与 Thunderbolt 3 兼容")与 Thunderbolt 3 兼容

Intel 向 USB Promoter 小组提供 Thunderbolt 3 协议时，曾发布消息称，他们会尽可能保证具备 USB 4 端口的设备同时兼容 Thunderbolt 3 和 USB 4 设备，这对通过连接 eGPU（外部图形卡）来玩游戏的笔记本电脑用户而言是个福音。

由于 Thunderbolt 是 Intel 标准，在任何 AMD 驱动的计算机上都没有实现这个标准，所以你会看到市面上有许许多多的 Thunderbolt 3 eGPU，但是却很少有笔记本或台式机适配了它。Thunderbolt 3 并非开放标准，它需要额外的芯片才能驱动，其实现成本比标准 USB 要高很多。因此，如果你想要一个 eGPU 或者一个超高速的 Thunderbolt 3 储存驱动器，你可以选择的计算机是十分有限的。

制造商使用 USB 4 标准不需要向 Intel 支付任何版税，因此大规模采用将成为可能。不过，这里有个问题，兼容 Thunderbolt 并非 USB 4 标准的必要部分，制造商不一定会实现它，这就会导致，你购买的带有 USB 4 接口的电脑，可能与你的 [Razer Core X](https://www.tomshardware.com/reviews/razer-core-x-egpu,5525.html) 图形底座不兼容。然而，USB Promoter 小组 CEO Brad Saunders 预计，大多数装有 USB 4 的电脑都会适配 Thunderbolt 3，

“我们电话联系了大部分的 PC 厂商，并表示殷切希望他们能够广泛向后兼容 Thunderbolt，目前他们大部分的设备已经将 USB 4 设计进去了，我预计大部分厂商都会接受我们的建议，但是也不排除有少数家伙忽略我们的建议。”

虽然 Saunders 对 PC 制造商在 USB 4 接口增加 Thunderbolt 3 兼容持乐观态度，但值得注意的是，任何厂商在推广时如果想说自家设备兼容 Thunderbolt 3 标准，都需要先经过 Intel 的认证，而这一认证过程十分严格，需要支付费用，所以不排除有部分厂商不兼容，或兼容得不够好（未经认证）。

### [](#USB-4-的三种速度 "USB 4 的三种速度")USB 4 的三种速度

理论上 USB 4 可以达到 40Gbps，然而并非所有的 USB 设备或主机支持这个协议。Saunders 告诉我们实际会有三种速度：10Gbps、20Gbps 和 40Gbps。一般小点、便宜点的设备，如手机、Chromebook 等，会实现较低的速度标准；如果你在购买笔记本电脑时，想获得更快的 USB 4 连接，建议你查看下规格。

### [](#在传输视频和数据时更好地共享带宽 "在传输视频和数据时更好地共享带宽")在传输视频和数据时更好地共享带宽

USB 4 标准的一个重要部分是，在同时连接传输视频和数据时，能够动态调整可用的资源量。假设你使用 40Gbps 带宽的 USB 4 从外部 SSD 复制大量文件（视频+数据）输出到 4K 显示器，如果视频源需要 12.5Gbps 的带宽，那么剩下的 27.5Gbps 带宽将分配给备份驱动器以传输数据。

USB\-C 引入了“替代模式”，即具备从 Type\-C 接口传输视频数据到 DisplayPort 或 HDMI 的能力，不过目前的 3.x 规范并没有提供分隔资源的好办法，根据 Saunders 的说法，DisplayPort 模式可以将 USB 数据和视频之间的带宽分成 1:1，而 HDMI 模式则根本不允许同时使用 USB 传输数据和视频。

“USB SuperSpeed 规范没有足够的灵活性来控制连接器以组合的方式来管理数据和视频这两种不同的带宽分配，而 USB 4 为在不同应用程序类型之间获得更多的伸缩性提供了优化。”

当前许多扩展坞并未采用 USB 4 技术，而是采用的 DisplayLink 技术，该技术会压缩视频信号并将其转换为 USB 数据。未来大多数 USB 4 设备是否会这么处理，还不好说。

### [](#所有的-USB-4-主机均支持-USB-供电器 "所有的 USB 4 主机均支持 USB 供电器")所有的 USB 4 主机均支持 USB 供电器

目前有一些用于高功率供电的 USB Type\-C 设备已经支持 USB 供电标准，但并非所有设备都支持。每个 USB 4 设备和主机都必须符合 USB 供电标准，这样可以实现更高的功率和更好的电源管理。

USB 供电理论上可以提供高达 100 瓦的功率，但充电设备不必支持这么大的功率也能正常充电。因此，我们无法保证所有的笔记本都能够提供 USB 4 所需要的额定功率来充电，但是我们可以呼吁它们遵循标准。

### [](#老设备向后兼容 "老设备向后兼容")老设备向后兼容

值得高兴的是，USB 4 可以与 USB 3 和 USB 2 设备或端口配合使用。但是，很显然，你只能在传输中获得较弱设备的速度和能力。比如，当你使用 USB 4 设备连接到 USB 3.2 端口时， USB 设备将无法以 40Gbps 的速度传输，在 USB 2 端口也是同样的道理，最快只会有 USB 2 端口的最高速度和传输能力。

### [](#你的旧数据线将以他们最快的速率工作 "你的旧数据线将以他们最快的速率工作")你的旧数据线将以他们最快的速率工作

![upload successful](https://www.barretlee.com/blogimgs/old-cables-in-usb-4.png)

你现有的 USB 数据线和适配器都可以和 USB 4 配合使用，但是与上面类似，它们只能以数据线的最高额定速度运行，比如你使用 3.1 版本的数据线（传输速度为 5Gbps），当你与 USB 4 端口相连时，也只能达到 5Gbps。另外，如果想获得 Thunderbolt 3 支持，你得使用 Thunderbolt 3 数据线。

### 不会来得很快

USB Promoter 论坛计划在 2019 年年中发布 USB 4 规范，Saunders 告诉我们，新产品的开发周期一般是 12~18 个月，所以你不要指望在 2020 年以前看到任何基于该标准实现的产品。

对于支持 USB 4 的笔记本电脑和台式机，即使 18 个月也只是保守估计。你看，Type\-C 的规格在 2014 年就宣布了，但是 USB Type\-C 花了很长的时间才成为主流，并且现在好多笔记本电脑依然没有实现 Type\-C 接口。

### 将比 USB 3.2 的制造成本更高

大规模采用 USB 4 的一个障碍是它的成本太高，目前我们还不清楚 PC 和设备供应商增加 USB 4 的确切成本，但我们知道它需要采用比当前最新标准（USB 3.2）更加昂贵的组件。

Saunders 说，“我希望成本能够迅速降低，但是，我们猜测成本差异会将 USB 4 推向更高端的 PC，至少在开始的时候是这样的。”

### 为什么官方称 USB 4 为“USB4”（无空格）

与过去版本的 USB 不同，新规范中的拼写少了一个空格，但是我依然相信大众还是会写成“USB 4”。USB Promoter 小组 CEO Brad Saunders 解释说，他期望删除空格可以让大家把重点放在版本号和品牌名称上。

“我想表达的是，未来我们并不打算引入 4.0、4.1、4.2 诸如此类的版本，保持简单就好”。

USB 3.x 规范有各种不同的版本，包括 USB 3.0、USB 3.1 Gen 1、USB 3.1 Gen 2 和 4 个不同版本的 USB 3.2，Saunders 期望未来在推广产品的时候，就用简单的诸如“SuperSpeed USB”这样的品牌，而不是带各种版本号。
