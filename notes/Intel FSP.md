---
title: Intel FSP
created: '2019-09-24T08:30:45.527Z'
modified: '2019-09-24T08:31:04.491Z'
---

# Intel FSP

## [X86生态圈为什么在物联网玩不转？什么是Intel® FSP ？它能解决什么问题？](https://zhuanlan.zhihu.com/p/83039006)

[![老狼](https://pic4.zhimg.com/50/v2-86ea4b8b7df16d9c219613abc03b1ee5_xs.jpg)](https://www.zhihu.com/people/mikewolfwoo)

[老狼](https://www.zhihu.com/people/mikewolfwoo)

[​](https://www.zhihu.com/question/48509984)

UEFI固件、服务器、嵌入式产品 (公众号:UEFIblog)

186 人也赞同了该文章

![X86生态圈为什么在物联网玩不转？什么是Intel® FSP ？它能解决什么问题？](https://pic2.zhimg.com/50/v2-260b6f0c7dd0611613d2e1f043d2f954_hd.jpg)

我在上篇文章中描述了BIOS在X86生态圈中的作用:

[老狼：UEFI 引导与 传统BIOS 引导在原理上有什么区别？芯片公司在其中扮演什么角色？​zhuanlan.zhihu.com ![图标](https://pic3.zhimg.com/v2-3953858af2db9082f40cc2a7d13b9062_180x120.jpg)](https://zhuanlan.zhihu.com/p/81960137)

x86生态圈，在PC以及服务器领域多年发挥的积极作用。然而在物联网，X86的推广却遇到了阻力，诚然这和X86价格高、虽然性能强，但在很多领域却不需要（over kill，杀鸡用牛刀）的因素有关，但与BIOS在物联网推广中的负面作用也有很大关系。为此Intel推出了FSP（Intel® Firmware Support Package）技术，经过多年推广，它帮助Intel在物联网领域积累了大批拥趸。FSP技术广受欢迎，并渐渐地向服务器领域扩散。今天我们就一起来了解一下X86生态圈的问题，以及FSP为什么能够解决它。

## X86生态圈

PC和服务器领域，主要的玩家芯片制造商（Intel，AMD），和OEM/ODM厂商多年合作无间。在他们之中，BIOS供应商（IBV，AMI/Insyde/Byosoft等）起到了关键的润滑剂和粘合剂的作用，它们三者的关系如下图：

![](https://pic1.zhimg.com/v2-809ea4e188e5fc80d62a8ccfece43883_b.jpg)

![](https://pic1.zhimg.com/80/v2-809ea4e188e5fc80d62a8ccfece43883_hd.jpg)

X86生态圈示意图

这里有三条主线，我用三种颜色标记。蓝色表示代码和硬件，芯片厂商一视同仁，同时提供给BIOS厂商和OEM参考代码和参考板（CRB，Custom Reference Board）。但参考代码并不代表产品代码，除非客户完全照抄参考板。实际上，OEM/ODM往往在参考板上有少许修改，这就需要修改参考代码。OEM因为产品线众多，需要大量BIOS人员修改参考代码。BIOS人员门槛较高，成本很高。所以OEM/ODM往往只在主力产品中采用自研，而将大量工作外包给**专业的**BIOS提供商（IBV）。BIOS供应商收到Intel的代码，和早期样片，就开始研发相关产品。多年浸淫BIOS，IBV往往能够很快搞定某代主板，因为同时为多家OEM服务，代码可以复用，也摊平了预研成本，搞定一个主板往往比OEM更便宜。

有句俗语讲"follow the money"，如果我们跟踪黄色的资金流，就更有意思了。BIOS厂商实际上免费得到的代码，经过自己加工，转手收费卖给了OEM。芯片制造商在BIOS中完全没有利润，只靠卖芯片赚钱。但因为可以免除自己动手支持众多OEM/ODM，也乐观其成。

芯片制造商、IBV和OEM，几十年合作下来相互非常熟悉，形成了稳定的三角形构建，帮助PC和服务器领域稳定健康发展。但这些到**崭新的**物联网领域却变得不那么和谐了。

## 物联网的挑战

物联网充斥了数量极其庞大的玩家，多数是从原来的嵌入式领域转过来的。如果说要总结它们的特征的话，就是**小、薄和快**。

小是指这些家伙往往体量相当小，几个人十几个人就可以开始开始搞物联网设备。“什么，要和Intel签NDA？”；“啥，还要给那个转手卖代码的给钱？Bootloader不是免费的吗？”这些闻所未闻的要求让他们傻眼了，凭什么uboot那么好不用，要交钱给IBV；签NDA，没有律师，想去拜山门都不知道门朝那里开。如此种种似乎把这些最具创新精神的小伙伴排除在外了。

薄是指底子薄。金钱少，技术底子薄。金钱少，意味着不能顾几个专职的BIOS工程师，也意味着不想给IBV钱，技术底子薄是指完全没有BIOS经验，不具备自研BIOS的可能。就算排除困难，签了NDA，Intel给BIOS/UEFI代码，从上文我们知道代码量相当大，也看不懂啊。

快，是指产品周期相当短。很多产品从Idea到产品最多有半年，甚至3个月。IBV不能保证这种支持频度。

失去了IBV，稳定的铁三角倾斜了：

![](https://pic1.zhimg.com/v2-6d1146c930abe2f0b1a0c8838d113030_b.jpg)

![](https://pic1.zhimg.com/80/v2-6d1146c930abe2f0b1a0c8838d113030_hd.jpg)

必须提供一种简单灵活的方法，让众多的物联网可以用它们熟悉的嵌入式模式：简单的代码，开放的bootloader，免费的芯片初始化代码。这样它们就可以专注于开发自己的产品，而不需要是BIOS专家。Intel给出的答案就是:FSP!

## Intel® FSP 是什么？

官方解释是：英特尔固件支持包（Intel® Firmware Support Package，简称为Intel® FSP）是由英特尔公司为支持其X86平台的芯片而发布的二进制格式文件。

似乎很难懂，简单来说就是Intel把芯片初始话代码封装成二进制文件，客户不需要知道里面做了什么，只要调用就行了。因为是二进制文件Binary，不会透露什么商业机密，所以不需要签NDA，Intel可以把它放在Github上给所有人免费下载。同时，这个调用接口相当简单，有多简单呢？只有5个API！

![](https://pic2.zhimg.com/v2-a291836dd58e199d43373bcd4f6fc64f_b.jpg)

![](https://pic2.zhimg.com/80/v2-a291836dd58e199d43373bcd4f6fc64f_hd.jpg)

5个API进去，1组包含结果的HOB出来。够简单吧！这其中的详情我就不展开了，有兴趣的可以去看看参考资料1[\[1\]](#ref_1)和参考资料2[\[2\]](#ref_2)。

这里简单介绍一下FSP的历史。它本来是用来支持Chromebook的。在Chromebook诞生之初，Google并不想用UEFI。其中原因有UEFI复杂，它更喜欢Linux Style之外，还有它对UEFI自带的微软血统有天生的敌视。于是脱胎于Linuxbios的coreboot被它提了出来，与之对应，它希望Intel用一个简单的东西封装芯片初始化代码，只提供必须的接口。而coreboot接管了UEFI的找到并初始化启动设备的功能。于是Intel提出了FSP，开始只有三个API

![](https://pic3.zhimg.com/v2-fa5c69b732443b4a879bec27f3b3d326_b.jpg)

![](https://pic3.zhimg.com/80/v2-fa5c69b732443b4a879bec27f3b3d326_hd.jpg)

这是FSP 1.1，经过多年改进现在是FSP 2.1。API加入了两个，并加入了对UEFI更友好的Dispatch mode（原来的模式被称为API mode）。

## FSP带来哪些好处？

别看中国的Chromebook卖不动（有不可说的原因），但在美国，Chromebook销量相当大。FSP也变得越来越重要，现在PC和笔记本的BIOS很多都已经基于FSP的了。尽管BIOS还是UEFI的，但芯片初始化都改成调用FSP的简单接口，这样UEFI本身也只是bootloader而已。这是因为FSP可以让BIOS开发更快：

![](https://pic1.zhimg.com/v2-fff1ad29803a6a49be5c07e2d47643d7_b.jpg)

![](https://pic1.zhimg.com/80/v2-fff1ad29803a6a49be5c07e2d47643d7_hd.jpg)

原本的开发模式

![](https://pic3.zhimg.com/v2-ebdca0b487d55c065c3fbefd52784de4_b.jpg)

![](https://pic3.zhimg.com/80/v2-ebdca0b487d55c065c3fbefd52784de4_hd.jpg)

基于FSP的开发模式

FSP的开放下载也为使用Open Source的Bootloader，如Coreboot，uboot等扫清了最后的障碍。于是，物联网领域里所当然的开始使用FSP。FSP的开放，让它可以支持几乎所有的bootloader：

![](https://pic2.zhimg.com/v2-310b33cbdd81fe2cee8468acf7804836_b.jpg)

![](https://pic2.zhimg.com/80/v2-310b33cbdd81fe2cee8468acf7804836_hd.jpg)

是的 ，你没有看错，还包括最新的LinuxBoot（注意区分它和Linuxbios）。这样，物联网的小伙伴们遇到的三个问题都解决了，大家又可以一起愉快的玩耍了。

## 后记

服务器领域，几大云服务器厂商也开始尝试摆脱IBV的束缚，自研BIOS。它们几乎同时看中了FSP，希望FSP搞定芯片初始化，它们自己借助coreboot，或者最近推出的LinuxBoot，来启动服务器。成本到不是考量的重点，重要的是可控性和TTM。FSP将来大有可为，但归根结底，FSP里面包含了一个小的EDKII系统（包括PeiCore和各种PEIM），UEFI也可以作为Bootloader起到。它的出现，将IP相关代码封装成binary，必然推动更多的UEFI平台开源，于此同时也会简化UEFI代码库，让UEFI也走向简单。

更多BIOS文章：

[老狼：UEFI背后的历史​zhuanlan.zhihu.com![图标](https://pic1.zhimg.com/v2-31a24ab2dc5cd836a415a69bf6072450_180x120.jpg)](https://zhuanlan.zhihu.com/p/25281151) [老狼：UEFI架构 ​zhuanlan.zhihu.com ![图标](https://pic4.zhimg.com/v2-44450bb9ed511c362e5c3fe5488b556b_180x120.jpg)](https://zhuanlan.zhihu.com/p/25941528) [老狼：UEFI与硬件初始化​zhuanlan.zhihu.com![图标](https://pic2.zhimg.com/v2-37aca03d25fa7d2ef21b95de5449a82d_180x120.jpg)](https://zhuanlan.zhihu.com/p/25941340) [老狼：ACPI与UEFI ​zhuanlan.zhihu.com ![图标](https://pic3.zhimg.com/v2-7cfec805a93f8f977a9ab1ac18ebe3a6_180x120.jpg)](https://zhuanlan.zhihu.com/p/25893464) [老狼：UEFI和UEFI论坛​zhuanlan.zhihu.com![图标](https://pic1.zhimg.com/v2-31a24ab2dc5cd836a415a69bf6072450_180x120.jpg)](https://zhuanlan.zhihu.com/p/25676417) [老狼：UEFI安全启动 ​zhuanlan.zhihu.com ![图标](https://pic2.zhimg.com/v2-07210ac3f5c8e2c230d5268d817feaa9_180x120.jpg)](https://zhuanlan.zhihu.com/p/25279889) [老狼：FAT文件系统与UEFI​zhuanlan.zhihu.com![图标](https://pic4.zhimg.com/v2-dc4d41e98267dc248d344d777e50e4cf_180x120.jpg)](https://zhuanlan.zhihu.com/p/25992179) [老狼：UEFI 引导与 传统BIOS 引导在原理上有什么区别？芯片公司在其中扮演什么角色？ ​zhuanlan.zhihu.com ![图标](https://pic3.zhimg.com/v2-3953858af2db9082f40cc2a7d13b9062_180x120.jpg)](https://zhuanlan.zhihu.com/p/81960137)

欢迎大家关注本专栏和用微信扫描下方二维码加入微信公众号"UEFIBlog"，在那里有最新的文章。

![](https://pic2.zhimg.com/v2-121ecd3d4080deb1c557bf47dc00d246_b.jpg)

![](https://pic2.zhimg.com/80/v2-121ecd3d4080deb1c557bf47dc00d246_hd.jpg)

用微信扫描二维码加入UEFIBlog公众号

## 参考

1.  [^](#ref_1_0)FSP EDA [https://cdrdv2.intel.com/v1/dl/getContent/611786](https://cdrdv2.intel.com/v1/dl/getContent/611786)
2.  [^](#ref_2_0)FSP Homepage [https://www.intel.com/content/www/us/en/intelligent\-systems/intel\-firmware\-support\-package/intel\-fsp\-overview.html](https://www.intel.com/content/www/us/en/intelligent-systems/intel-firmware-support-package/intel-fsp-overview.html)

[编辑于 2019\-09\-19](https://zhuanlan.zhihu.com/p/83039006)

