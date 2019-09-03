---
title: "为什么很多硅谷工程师偏爱 OS X，而不是 Linux 或 Windows？" 
link: http://www.cocoachina.com/programmer/20160809/17318.html
date: 2016-08-09 07:17
---

[Why do most of the developers in Silicon Valley prefer OS X over Linux or Windows? ](https://www.quora.com/Why-do-most-of-the-developers-in-Silicon-Valley-prefer-OS-X-over-Linux-or-Windows)


题主还补充说：
「看过许多诸如 Google、Dropbox、Quora 等公司的视频，发现有超过 90% 的工程师都在使用 Mac。Mac 为什么这么流行？难道大家没发现下面这些问题嘛？」

- 昂贵但 GPU 性能一般！
- 没 Linux 开放（Linux 允许你编辑任何文件）
- 服务器是运行在 Linux 上的，在 Linux 上开发岂不更好。

本以为帖子的回复中会详细列出 Mac 的优点，却没想到一个偏爱 Linux 的程序员的回答收到了宠幸。

## Mike Mikowski 的回答，2700+ 顶

Jeff Dean，显然认为把 Linux 作为自己的首选操作系统，这是个非常好的主意。他在过去十年中为 Google 创造了数十亿美元的收入。

作为硅谷的一名高级架构师，Linux 和 OS X 我都在使用，但 Linux 更适合我。我坚信 Linux 应该被普及。说实话，OS X 在某些方面的确比 Linux 好，但这优势已经越来越小，因为 OS X 的缺陷越来越多了。

我相信，当工程师花 2500 美元买了一台 Macbook Retina Pro（16 GB 内存，Intel Iris Pro Graphics 图形处理器 和 AMD Radeon R9 M370X 图形处理器），就会发现 Mac 并没有比 Linux 优秀。随着 Mac 硬件（GPU）进一步落后和软件质量下降，我能预料 Linux 的普及。

让 Linux 普及的要点是可定制以及厂家的支持。这将解决硬件兼容性问题，启动时再也不会出现“just some old hardware”的提示。我现在用 System76 的笔记本和台式电脑，他们的电脑和售后非常好。我其中的一台笔记本 Oryx Pro 配置如下：搭载 GSync 技术的 GTX970m 显卡，固态硬盘， i7 处理器，32 GB 内存和 IPS 显示器，一点也不比 Macbook Pro 差。同时，我维护着一份 Kubuntu 下装机必备软件列表，欢迎评论和补充。

以下是我为什么用 Linux 开发和个人电脑的原因：

1. 强劲的 GPU 和 CPU。我的家用电脑和笔记本都配了顶级的显示器和 GPU。Steam 上有 2000 +游戏，我和孩子玩了很多，并且我对 CUDA 和 深度学习很感兴趣。而 Mac 对此就无能为力了。对我来说，强大的 GPU 是非常重要的，所以我配了一台搭载 GTX980Ti 显卡、32 GB 内存、3440×1440分辨率的曲面屏、i7 WildDogPro 处理器的电脑。而价格跟 MBP Retina 的价格相差无几。

2. 与服务器真正的兼容。我曾工作过的一家公司，半数脚本是用 Linux 脚本和 GNU 工具写的，另一半是 Bash 和 BSD 工具写的，因为这部分开发者坚持用 Mac 开发，并认为 Mac 和 Linux 相差无几。我只想说，这真的太乱了。后来，我花了几周时间重写了代码（为了兼容）。这些人一边说：“Mac OS X 是不同的”，但另一边又说：“Mac OS X 跟 Linux 相差无几”。但事实并不是，Mac OS X 与 Linux 差很多。

3. Linux 使我更高效。几乎不需要切换环境，我可以使用相同的工具，专注编写脚本，与服务器无缝衔接。至今，我还没发现哪个 Mac 用户能展现出令我印象深刻的 脚本编写能力。我曾经管理过一个需要超过 100 台以上 Linux 服务器集群的软件，除了 Linux ，其它系统根本胜任不了这个任务。

4. Linux 上一切允许正常（Everything just works）。相对那些用 Mac 的同事，我在 Linux 上碰到的问题更少。不需要安装 homebrew、fink、MacPorts，只要运行 suao apt-get update 和 sudo apt-get upgrade 这两个命令就可以了。团队里有少数人还在坚持使用 Mac，他们认为这是一种荣耀。编写脚本和程序设置电脑， Mac 大概要 2-3 天，而 Linux 只要一个小时，而且几乎是全自动化的。

5. Linux 能做 Mac 不能做的事。有一天，我们需要超高清分辨率的屏幕截图，在 Linux 上只要输入: $ xrandr --output DP-2.8 -s 3440x2880 --panning 3440x2880 ，之后用 Chrome 把图片放大一倍，并用 Ksnapshot 获得所需图片，前后只需要几分钟。谁能在 Mac 或者 Windows 上实现？

6. “Linux 上不能运行商业软件”这个争论是毫无意义的，尤其是在硅谷，这里早就不使用微软的 Office 软件了。我使用 Balsamiq 3、Slack、DropBox、Chrome、Hangout、Google 办公套件、GoToMeeting、WebStorm、Skype、Gimp、Insync等软件…对了，在休息时间我会在 Steam 上玩会游戏。所有程序都运行良好，几乎没有应用程序或者系统崩溃。Webstorm（JetBrains IDE）在 Linux 上更友好，同样，NodeJS、MongoDB、MySQL和其它一系列开发工具在 Linux 都比 Mac 友好。

KED/Plasma 的界面比 Mac 和 Windows 都友好多了。你问我为什么这么说，因为 KDE/Plasma 我都在使用。

这么多开发人员缺乏系统管理和使用脚本的技能，真是一件令人沮丧的事。相对命令行来说，Mac OS X 的图形界面限制了程序员的思考。如果有一天我在硅谷创业，我会把 Linux 当做首选的操作系统。

注：

CUDA：一种由 NVIDIA 推出的通用并行计算架构，该架构使GPU能够解决复杂的计算问题。
deep learning：深度学习。源于人工神经网络的研究。含多隐层的多层感知器就是一种深度学习结构。深度学习通过组合低层特征形成更加抽象的高层表示属性类别或特征，以发现数据的分布式特征表示。需要 N 卡支持，而 Macbook 基本上配的都是 Intel 显卡。

## Garry Taloy 的回答，103 顶

GPU:

程序员 != 游戏玩家。

毫无疑问，配有更快的 GPU 显示效果更好，但除非你是开发游戏或者需要进行 3D 渲染，否则太奢侈了。我自己的两台开发用电脑，都只配备了普通的 Intel 集成显卡。

Linux 允许你修改任何文件：

我只是一位程序员，我只关心自己的代码，除此之外，别的文件能否修改和编译我并不关心。或许你该问问自己：什么是我真正需要编辑的？我是否有能力去修改文件？我为什么要这么做？

在同一个系统下开发更好（代指服务器环境和开发环境相同）

首先，这个理由相当不错，但对于 Apache 或者其它什么，在 Mac OS X 上和 Linux 上运行的效果是极其相似的。如果使用诸如 OpenVMS 、 IBM i 或者 z/OS 这些操作系统，那就另当别论了。但是，我敢打赌，在 Mac 上运行网页的效果肯定跟 Linux 是一样的。

Expensive:

贵

实则不然，在相似情况下，一台搭载高清屏、 SSD 固态硬盘 、工业设计都不错的、你听过的牌子的笔记本， Mac 的价格其实并没有贵多少。姑且就说 Mac 贵几百美元吧，但能给程序员支付高薪的公司，并不会吝啬这部分钱。假设 Google 程序员的年薪是 100,000 美元，加上税收和医疗保险、办公空间等额外支出，Google 每年在程序员身上的花费可能会达到 150000 美元。你认为 Google 会在乎多花 300 美元购买 Mac 嘛？

这就好像花 10 美元买喜欢的披萨，和话 10.01 美元买你很喜欢的披萨，没多少差别~

回答你的问题，就要忽略你说的，因为你说的 99% 与问题无关。

## Ivan Apple 的回答

### 服务器是运行在 Linux 上的，在 Linux 上开发岂不更好

兄弟，你这让我想起这个论点还有价值的年代。想当年，想用 Linux 却没钱买新电脑，唯一的选择就是将 Linux 作为你的主操作系统（学会忍受糟糕的 GUI、应用少等这些不足之处）。

如今，通过 VirtualBox，可以很轻松地在 Windows 或 Mac 上安装 Linux 服务器发行版，“在同一个操作系统下开发”就迎刃而解了。

最终，学习如何调整 KDE Plasma 和 分析 Wi-Fi 驱动并不会让你成为一个更好的程序员和系统管理员。这只是一个习惯问题，而且是相当无聊的习惯。

### Linux 允许你修改任何文件

这个观点经常出现在争论中。

我就说一句话：Ubuntu 和 Red Hat 是现实世界中最流行的 Linux 发行版，而它们可以在 VirtualBox 很好的工作。

### 昂贵但 GPU 性能一般！

其实 Mac 并不贵。MB Pro 大概只要 1300 欧元（折合人民币约 9500 元）。而差不多配置 HP 或者 Lenove 笔记本大概要 900 欧元（折合人民币约 6600元）。根据笔记本平均生命周期，购买外形轻薄、耐用（强劲的电池寿命）的 Mac 每个月只要多支付 7 欧元即可。这是一个很划算的投资。
