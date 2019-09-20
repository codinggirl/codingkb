素材
fodder
material
matter

实际生活中未经总结提炼的形象,文学、艺术的原始材料


## 图片右对齐

| \\begin{flushright}
\\end{flushright} |

| \\raggedleft
在 figure 环境中不要用 flushright 环境，这会造成多余的空白。 |


| 图片的话还是用 \\raggedright, \\centering 和 \\raggedleft 吧，环境用列表会导致额外的 vertical space。 |



很多论文首页中需要在题目和作者处加入标注，并在脚注中引用，注明作者的地址或者一些附注信息。比如下例:

[![Latex标题页的上标和脚注](http://s1.sinaimg.cn/mw690/531bb763gd2c0b188a900&690 "Latex标题页的上标和脚注")](http://photo.blog.sina.com.cn/showpic.html#blogid=531bb763010185vk&url=http://s1.sinaimg.cn/orignal/531bb763gd2c0b188a900)

实现这种效果一般可采用Latex中的\\thanks命令，调用格式为

author\\thanks{text}

但是\\thanks对标注的编号是自动的，无法重复编号。比如说，author1和author2的标注应当都是1，在两者后面加\\thanks，得到的效果是author1的标注是1，而author2的标注是2。

另外一种解决方案是使用

\\author\[label\]{author name}

\\address\[label\]{address text}

这种组合调用方式。这样通过label引用，可以实现多重标注和标号的重复。缺点是，很多模板或者sty包会于其冲突或不支持该命令。另外很多模板和环境（如box环境等）中\\thanks也无法正常调用。

终极解决方案：

\\thanks的本质其实等价于两个命令\\footnotemark和\\footnotetext，调用格式为

\\footnotemark\[num\]

\\footnotetext\[num\]{text}

\\footnotemark\[num\]，这里num指示使用当前mark的第几个符号，mark包括阿拉伯数字、罗马数字、特殊符号等多种标识符。

\\footnotetext\[num\]{text}，这里的num和上面的num不同，指示的是footnotetext的排列顺序。

下面给出一个具体的例子：

\\documentclass\[11pt,a4paper\]{article}

\\begin{document}

\\title{\\Large \\bf How use $\\backslash$footnotemark and $\\backslash$footnotetext \\footnotemark\[1\]}

\\author{

Author1

\\footnotemark\[2\],

Author2

\\footnotemark\[2\]

\\footnotemark\[3\],

Author3\\footnotemark\[4\],

Author4\\footnotemark\[4\]}

\\renewcommand{\\thefootnote}{\\fnsymbol{footnote}}

\\footnotetext\[1\]{You can add acknowledgements here.}

\\footnotetext\[2\]{Address of Author1}

\\footnotetext\[3\]{Address ofAuthor2}

\\footnotetext\[4\]{Address of Author3 and Author4}

\\maketitle

\\begin{abstract}

Abstract.

\\end{abstract}

\\end{document}

[Latex标题页的上标和脚注_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb763010185vk.html)



## Latex表格的尾注和脚注

 ![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif "此博文包含图片") (2013\-01\-05 22:04:14)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [Computer](http://blog.sina.com.cn/s/articlelist_1394325347_7_1.html) |

见代码：

\\documentclass\[10pt\]{article}
\\usepackage{booktabs}
\\usepackage{threeparttable}
\\begin{document}

\\begin{table}\[htbp\]
\\centering
\\small
\\begin{threeparttable}
\\caption{\\label{tab:results}Effect of Trade Openness on Environment (Air Pollution)}
\\begin{tabular}{lccc}
\\toprule
& NO $ \_2 $ & SO $ \_2 $ & PM \\\\\\midrule
$\\ln(y/pop)$ & 408.74\* & 287.25\* & 566.65 \\\\& (121.79) & (118.81) & (336.19)\\\\
$\\ln(y/pop)^2$ & $\-$22.85\* & $\-$16.58\* & $\-$35.57\*\*
\\\\& (6.90) & (6.78) & (19.06)
\\\\$(X+M)/Y$ & $\-$.29\*\* & $\-$.31\* & $\-$.37
\\\\& (.17) & (.08) & (.34)
\\\\$Polity$ & $\-$3.20\* & $\-$6.58\* & $\-$6.70\*\*
\\\\& (1.47) & (2.05) & (3.42)
\\\\$\\ln(LandArea/pop)$ & $\-$5.94 & $\-$2.92\* & $\-$13.02\*
\\\\& (5.93) & (1.39) & (6.29)
\\\\Obs. & 36 & 41 & 38
\\\\$R^2$ & 0.16 & 0.68 & 0.62
\\\\
\\bottomrule
\\end{tabular}
%尾注
\\small Note: Robust standard errors in parentheses. Intercept included but not reported.
%脚注
\\begin{tablenotes}
\\item\[\*\] significant at 5
\\% level
\\item\[\*\*\] significant at 10
\\% level
\\end{tablenotes}
\\end{threeparttable}
\\end{table}

\\end{document}

注意：要用到的包。

结果图如下：

[Latex表格的尾注和脚注_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb76301018488.html)



## LaTex表格内单元格内容强制换行

 ![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif "此博文包含图片") (2013\-01\-05 16:30:04)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [Computer](http://blog.sina.com.cn/s/articlelist_1394325347_7_1.html) |

/newcommand{/tabincell}\[2\]{/begin{tabular}{@{}#1@{}}#2/end{tabular}}%放在导言区
%然后使用&/tabincell{c}{}&就可以在表格中自动换行

%比如这么用
/begin{tabular}{|c|c|}
/hline
1 & the first line //
/hline
2 & /tabincell{c}{haha// heihei//zeze} //
/hline
/end{tabular}

以下为一例子，可直接存为.tex文件编译运行:

\\documentclass\[a4paper,12pt\]{article}
\\begin{document}
\\begin{table}
\\newcommand{\\tabincell}\[2\]{\\begin{tabular}{@{}#1@{}}#2\\end{tabular}}
   \\centering
   \\begin{tabular}{|c|c|c|}\\hline
1 & \\tabincell{c}{the first line \\\\ the next\\\\the next\\\\ last} & \\tabincell{c}{one \\\\ one}\\\\\\hline
2 & \\tabincell{c}{hello\\\\ aha\\\\ ok [\\\\yes](file:////yes) [\\\\en](file:////en)} & \\tabincell{c}{two \\\\ two \\\\ two} \\\\\\hline
\\end{tabular}
   \\caption{longtitle}
\\end{table}

\\end{document}

结果如下图：


[LaTex表格内单元格内容强制换行_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb7630101841e.html)



## LaTex表格横置

 ![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif "此博文包含图片") (2013\-01\-05 10:24:43)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [Computer](http://blog.sina.com.cn/s/articlelist_1394325347_7_1.html) |

**\\usepackage{rotating} 宏包提供了 \\begin{sidewaystable} \\end{sidewaystable}环境.表格就会产生旋转，若是想让页面横置，需要**[**Landscape  环境**](http://www.ctex.org/documents/latex/graphics/node94.html)

\-\-\-\-\-\-\-\-\-\-\- Example 2 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
\\begin{sidewaystable}\[h\]
\\caption{Performance After Post Filtering} % title name of the table
\\centering % centering table
\\begin{tabular}{l c c rrrrrrr} % creating 10 columns
\\hline\\hline % inserting double\-line
Audio &Audibility & Decision &\\multicolumn{7}{c}{Sum of Extracted Bits}
\\\\ \[0.5ex\]
\\hline % inserts single\-line
% Entering 1st row
& &soft &1 & $\-1$ & 1 & 1 & $\-1$ & $\-1$ & 1 \\\\\[\-1ex\]
\\raisebox{1.5ex}{Police} & \\raisebox{1.5ex}{5}&hard
& 2 & $\-4$ & 4 & 4 & $\-2$ & $\-4$ & 4 \\\\\[1ex\]
% Entering 2nd row
& &soft & 1 & $\-1$ & 1 & 1 & $\-1$ & $\-1$ & 1 \\\\\[\-1ex\]
\\raisebox{1.5ex}{Beethoven} & \\raisebox{1.5ex}{5}& hard
&8 & $\-8$ & 2 & 8 & $\-8$ & $\-8$ & 6 \\\\\[1ex\]
% Entering 3rd row
& &soft & 1 & $\-1$ & 1 & 1 & $\-1$ & $\-1$ & 1 \\\\\[\-1ex\]
\\raisebox{1.5ex}{Metallica} & \\raisebox{1.5ex}{5}& hard
&4 & $\-8$ & 8 & 4 & $\-8$ & $\-8$ & 8 \\\\\[1ex\]
% \[1ex\] adds vertical space
\\hline % inserts single\-line
\\end{tabular}
\\label{tab:LPer}
\\end{sidewaystable}
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-end\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
[ ](http://s4.sinaimg.cn/middle/5e16f17749992970ccdd3&690)[![LaTex表格横置](http://s15.sinaimg.cn/mw690/531bb763gd28154a4f07e&690 "LaTex表格横置")](http://photo.blog.sina.com.cn/showpic.html#blogid=531bb763010183rs&url=http://s15.sinaimg.cn/orignal/531bb763gd28154a4f07e)

有一个问题需要注意，选自[http://bbs.ctex.org/viewthread.php?tid=41564](http://bbs.ctex.org/viewthread.php?tid=41564 "http://bbs.ctex.org/viewthread.php?tid=41564")

我用的是book，要画横向的大表，不论是在奇数页或是偶数页，都希望表头能在左边，即逆时针旋转90度。但是现在我直接用默认的sidewaystable后，奇数页的表没问题，但偶数页的表表头都在右边，即顺时针旋转了90度，请问应该如何修正呢？

【解决方案】

现在明白了，应该在用rotating的时候写：
\\usepackage\[figuresright\]{rotating}
这样表底或是图注就都会向右，表头向左了。


[LaTex表格横置_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb763010183rs.html)


## Latex设置表格字体大小

(2013\-01\-04 11:09:48)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [Computer](http://blog.sina.com.cn/s/articlelist_1394325347_7_1.html) |

Latex设置表格字体大小

\\begin{table}\[h\]

\\**small** % 此处写字体大小控制命令

\\begin{tabular}

\\end{tabular}

\\end{table}

Latex 设置字体大小命令由小到大依次为：

\\tiny

\\scriptsize

\\footnotesize

\\small

\\normalsize

\\large

\\Large

\\LARGE

\\huge

\\Huge


[Latex设置表格字体大小_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb7630101833m.html)



## LaTeX 中更改单个页面页边距

 ![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif "此博文包含图片") (2013\-01\-04 10:34:02)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [Computer](http://blog.sina.com.cn/s/articlelist_1394325347_7_1.html) |

当文档中存在尺寸比较大的图片时，为了将部分图片放到一个页面内，可能会需要调整单个页面的页边距。版本5之后的gemoetry宏包所包含的\\newgeometry命令可以实现这一目的。具体的方法很多，这里只介绍我认为最简单的一个。

当然，首先需要在导言区调用geometry宏包：

\\usepackage{geometry}

然后在需要调整页边距的页面，加入下面命令：

\\newgeometry{left=3cm,bottom=1cm}

这样就把左边距和下边距分别调整为3cm和1cm，右边距和上边距则保持不变。

如果需要恢复到原来的页边距，用下面的命令：

\\restoregeometry

##################################################################################################

使用geometry宏包，可以让页边距和页眉页脚的设置变得非常简单

\\documentclass\[a4paper\]{article}

\\usepackage{geometry}

\\geometry{left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm}

\\begin{document}

test

\\end{document}

常用的长度选项还有head, headsep, foot，见下图


[LaTeX 中更改单个页面页边距_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb7630101832g.html)




## CentOS性能监控工具

(2012\-07\-05 09:13:16)

[![](http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif)转载*▼*](javascript:;)

| 标签：

### [杂谈](http://search.sina.com.cn/?c=blog&q=%D4%D3%CC%B8&by=tag)

 | 分类： [LinuxAndCPP](http://blog.sina.com.cn/s/articlelist_1394325347_11_1.html) |

Linux系统出现问题时，我们不仅需要查看系统日志信息，而且还要使用大量的性能监测工具来判断究竟是哪一部分（内存、CPU、硬盘……）出了问题。在Linux系统中，所有的运行参数保存在虚拟目录/proc中，换句话说，我们使用的性能监控工具取到的数据值实际上就是源自于这个目录，当涉及到系统高估时，我们就可以修改/proc目录中的相关参数了，当然有些是不能乱改的。下面就让我们了解一下这些常用的性能监控工具。

| 工具 |

功能描述

 |
|

uptime

 |

系统平均负载率

 |
|

dmesg

 |

硬件/系统信息

 |
|

top

 |

进程进行状态

 |
|

iostat

 |

CPU和磁盘平均使用率

 |
|

vmstat

 |

系统运行状态

 |
|

sar

 |

实时收集系统使用状态

 |
|

KDE System Guard

 |

图形监控工具

 |
|

free

 |

内存使用率

 |
|

traffic\-vis

 |

网络监控（只有SUSE有）

 |
|

pmap

 |

进程内存占用率

 |
|

strace

 |

追踪程序运行状态

 |
|

ulimit

 |

系统资源使用限制

 |
|

mpstat

 |

多处理器使用率

 |

**1****、****uptime**

uptime命令用于查看服务器运行了多长时间以及有多少个用户登录，快速获知服务器的负荷情况。

uptime的输出包含一项内容是load average，显示了最近1，5，15分钟的负荷情况。它的值代表等待CPU处理的进程数，如果CPU没有时间处理这些进程，load average值会升高；反之则会降低。
load average的最佳值是1，说明每个进程都可以马上处理并且没有CPU cycles被丢失。对于单CPU的机器， 1或者2是可以接受的值；对于多路CPU的机器，load average值可能在8到10之间。
也可以使用uptime命令来判断网络性能。例如，某个网络应用性能很低，通过运行uptime查看服务器的负荷是否很高，如果不是，那么问题应该是网络方面造成的。
以下是uptime的运行实例：
9:24am  up  19:06,  1 user,  load average: 0.00, 0.00, 0.00
也可以查看/proc/loadavg和/proc/uptime两个文件，注意不能编辑/proc中的文件，要用cat等命令来查看，如：
liyawei:~ # cat /proc/loadavg
0.00 0.00 0.00 1/55 5505
**
2****、****dmesg**

dmesg命令主要用来显示内核信息。使用dmesg可以有效诊断机器硬件故障或者添加硬件出现的问题。
另外，使用dmesg可以确定您的服务器安装了那些硬件。每次系统重启，系统都会检查所有硬件并将信息记录下来。执行/bin/dmesg命令可以查看该记录。
dmesg输入实例：
ReiserFS: hda6: checking transaction log (hda6)
ReiserFS: hda6: Using r5 hash to sort names
Adding 1044184k swap on /dev/hda5.  Priority:\-1 extents:1 across:1044184k
parport\_pc: VIA 686A/8231 detected
parport\_pc: probing current configuration
parport\_pc: Current parallel port base: 0x378
parport0: PC\-style at 0x378 (0x778), irq 7, using FIFO \[PCSPP,TRISTATE,COMPAT,ECP\]
parport\_pc: VIA parallel port: io=0x378, irq=7
lp0: using parport0 (interrupt\-driven).
e100: Intel(R) PRO/100 Network Driver, 3.5.10\-k2\-NAPI
e100: Copyright(c) 1999\-2005 Intel Corporation
ACPI: PCI Interrupt 0000:00:0d.0\[A\] \-> GSI 17 (level, low) \-> IRQ 169
e100: eth0: e100\_probe: addr 0xd8042000, irq 169, MAC addr 00:02:55:1E:35:91
usbcore: registered new driver usbfs
usbcore: registered new driver hub
hdc: ATAPI 48X CD\-ROM drive, 128kB Cache, UDMA(33)
Uniform CD\-ROM driver Revision: 3.20
USB Universal Host Controller Interface driver v2.3
**
3****、****top**

top命令显示处理器的活动状况。缺省情况下，显示占用CPU最多的任务，并且每隔5秒钟做一次刷新。
Process priority的数值决定了CPU处理进程的顺序。LIUNX内核会根据需要调整该数值的大小。nice value局限于priority。 priority的值不能低于nice value（nice value值越低，优先级越高）。 您不可以直接修改Process priority的值，但是可以通过调整nice level值来间接地改变Process priority值，然而这一方法并不是所有时候都可用。 如果某个进程运行异常的慢，可以通过降低nice level为该进程分配更多的CPU。
Linux 支持的 nice levels 由19 (优先级低)到\-20 (优先级高)，缺省值为0。
执行/bin/ps命令可以查看到当前进程的情况。
**
4****、****iostat**

iostat由Red Hat Enterprise Linux AS发布。同时iostat也是Sysstat的一部分，可以下载到，网址是[http://perso.wanadoo.fr/sebastien.godard/](http://perso.wanadoo.fr/sebastien.godard/)
执行iostat命令可以从系统启动之后的CPU平均时间，类似于uptime。除此之外，iostat还对创建一个服务器磁盘子系统的活动报告。该报告包含两部分：CPU使用情况和磁盘使用情况。
iostat显示实例：
avg\-cpu:  %user   %nice %system %iowait  %steal   %idle
0.16    0.01    0.03    0.10    0.00   99.71

Device:            tps   Blk\_read/s   Blk\_wrtn/s   Blk\_read   Blk\_wrtn
hda               0.31         4.65         4.12     327796     290832

avg\-cpu:  %user   %nice %system %iowait  %steal   %idle
1.00    0.00    0.00    0.00    0.00  100.00

Device:            tps   Blk\_read/s   Blk\_wrtn/s   Blk\_read   Blk\_wrtn
hda               0.00         0.00         0.00          0          0

avg\-cpu:  %user   %nice %system %iowait  %steal   %idle
0.00    0.00    0.00    0.00    0.00   99.01

Device:            tps   Blk\_read/s   Blk\_wrtn/s   Blk\_read   Blk\_wrtn
hda               0.00         0.00         0.00          0          0
CPU占用情况包括四块内容
%user：显示user level (applications)时，CPU的占用情况。
%nice：显示user level在nice priority时，CPU的占用情况。
%sys:显示system level (kernel)时，CPU的占用情况。
%idle: 显示CPU空闲时间所占比例。

磁盘使用报告分成以下几个部分：
Device: 块设备的名字
tps: 该设备每秒I/O传输的次数。多个I/O请求可以组合为一个，每个I/O请求传输的字节数不同，因此可以将多个I/O请求合并为一个。
Blk\_read/s, Blk\_wrtn/s: 表示从该设备每秒读写的数据块数量。块的大小可以不同，如1024, 2048 或 4048字节，这取决于partition的大小。

例如，执行下列命令获得设备/dev/sda1 的数据块大小：
dumpe2fs \-h /dev/sda1 |grep \-F "Block size"

输出结果如下
dumpe2fs 1.34 (25\-Jul\-2003)
Block size: 1024

Blk\_read, Blk\_wrtn: 指示自从系统启动之后数据块读/写的合计数。
也可以查看这几个文件/proc/stat，/proc/partitions，/proc/diskstats的内容。
**
5****、****vmstat**

vmstat提供了processes, memory, paging, block I/O, traps和CPU的活动状况
procs \-\-\-\-\-\-\-\-\-\-\-memory\-\-\-\-\-\-\-\-\-\- \-\-\-swap\-\- \-\-\-\-\-io\-\-\-\- \-system\-\- \-\-\-\-\-cpu\-\-\-\-\-\-
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0      0 513072  52324 162404    0    0     2     2  261   32  0  0 100  0  0
0  0      0 513072  52324 162404    0    0     0     0  271   43  0  0 100  0  0
0  0      0 513072  52324 162404    0    0     0     0  255   27  0  0 100  0  0
0  0      0 513072  52324 162404    0    0     0    28  275   51  0  0 97  3  0
0  0      0 513072  52324 162404    0    0     0     0  255   21  0  0 100  0  0
各输出列的含义：
Process
– r: The number of processes waiting for runtime.
– b: The number of processes in uninterruptable sleep.
Memory
– swpd: The amount of virtual memory used (KB).
– free: The amount of idle memory (KB).
– buff: The amount of memory used as buffers (KB).
Swap
– si: Amount of memory swapped from the disk (KBps).
– so: Amount of memory swapped to the disk (KBps).
IO
– bi: Blocks sent to a block device (blocks/s).
– bo: Blocks received from a block device (blocks/s).
System
– in: The number of interrupts per second, including the clock.
– cs: The number of context switches per second.
CPU (these are percentages of total CPU time)
\- us: Time spent running non\-kernel code (user time, including nice time).
– sy: Time spent running kernel code (system time).
– id: Time spent idle. Prior to Linux 2.5.41, this included IO\-wait time.
– wa: Time spent waiting for IO. Prior to Linux 2.5.41, this appeared as zero.
**
6****、****sar**

sar是Red Hat Enterprise Linux AS发行的一个工具，同时也是Sysstat工具集的命令之一，可以从以下网址下载：[http://perso.wanadoo.fr/sebastien.godard/](http://perso.wanadoo.fr/sebastien.godard/)
sar用于收集、报告或者保存系统活动信息。sar由三个应用组成：sar显示数据、sar1和sar2用于收集和保存数据。
使用sar1和sar2，系统能够配置成自动抓取信息和日志，以备分析使用。配置举例：在/etc/crontab中添加如下几行内容
同样的，你也可以在命令行方式下使用sar运行实时报告。如图所示：
从收集的信息中，可以得到详细的CPU使用情况(%user, %nice, %system, %idle)、内存页面调度、网络I/O、进程活动、块设备活动、以及interrupts/second
liyawei:~ # sar \-u 3 10
Linux 2.6.16.21\-0.8\-default (liyawei)   05/31/07

10:17:16          CPU     %user     %nice   %system   %iowait     %idle
10:17:19          all      0.00      0.00      0.00      0.00    100.00
10:17:22          all      0.00      0.00      0.00      0.33     99.67
10:17:25          all      0.00      0.00      0.00      0.00    100.00
10:17:28          all      0.00      0.00      0.00      0.00    100.00
10:17:31          all      0.00      0.00      0.00      0.00    100.00
10:17:34          all      0.00      0.00      0.00      0.00    100.00
**
7****、****KDE System Guard**

KDE System Guard (KSysguard) 是KDE图形方式的任务管理和性能监视工具。监视本地及远程客户端/服务器架构体系的中的主机。
**
8****、****free**

/bin/free命令显示所有空闲的和使用的内存数量，包括swap。同时也包含内核使用的缓存。
total       used       free     shared    buffers     cached
Mem:        776492     263480     513012          0      52332     162504
\-/+ buffers/cache:      48644     727848
Swap:      1044184          0    1044184
**
9****、****Traffic\-vis**

Traffic\-vis是一套测定哪些主机在IP网进行通信、通信的目标主机以及传输的数据量。并输出纯文本、HTML或者GIF格式的报告。

注：Traffic\-vis仅仅适用于SUSE LINUX ENTERPRISE SERVER。

如下命令用来收集网口eth0的信息：
traffic\-collector \-i eth0 \-s /root/output\_traffic\-collector
可以使用killall命令来控制该进程。如果要将报告写入磁盘，可使用如下命令：
killall \-9 traffic\-collector
要停止对信息的收集，执行如下命令：killall \-9 traffic\-collector

注意，不要忘记执行最后一条命令，否则会因为内存占用而影响性能。

可以根据packets, bytes, TCP连接数对输出进行排序，根据每项的总数或者收/发的数量进行。
例如根据主机上packets的收/发数量排序，执行命令：
traffic\-sort \-i output\_traffic\-collector \-o output\_traffic\-sort \-Hp

如要生成HTML格式的报告，显示传输的字节数，packets的记录、全部TCP连接请求和网络中每台服务器的信息，请运行命令：
traffic\-tohtml \-i output\_traffic\-sort \-o output\_traffic\-tohtml.html
如要生成GIF格式（600X600）的报告，请运行命令：
traffic\-togif \-i output\_traffic\-sort \-o output\_traffic\-togif.gif \-x 600 \-y 600

GIF格式的报告可以方便地发现网络广播，查看哪台主机在TCP网络中使用IPX/SPX协议并隔离网络，需要记住的是，IPX是基于广播包的协议。如果我们需要查明例如网卡故障或重复IP的问题，需要使用特殊的工具。例如SUSE LINUX Enterprise Server自带的Ethereal。
技巧和提示：使用管道，可以只需执行一条命令来产生报告。如生成HTML的报告，执行命令：
cat output\_traffic\-collector | traffic\-sort \-Hp | traffic\-tohtml \-o output\_traffic\-tohtml.html
如要生成GIF文件，执行命令：
cat output\_traffic\-collector | traffic\-sort \-Hp | traffic\-togif \-o output\_traffic\-togif.gif \-x 600 \-y 600
**
10****、****pmap**

pmap可以报告某个或多个进程的内存使用情况。使用pmap判断主机中哪个进程因占用过多内存导致内存瓶颈。
pmap <pid>

liyawei:~ # pmap  1
1: init
START       SIZE     RSS   DIRTY PERM MAPPING
08048000    484K    244K      0K r\-xp /sbin/init
080c1000      4K      4K      4K rw\-p /sbin/init
080c2000    144K     24K     24K rw\-p \[heap\]
bfb5b000     84K     12K     12K rw\-p \[stack\]
ffffe000      4K      0K      0K \-\-\-p \[vdso\]
Total:      720K    284K     40K

232K writable\-private, 488K readonly\-private, and 0K shared
**
11****、****strace**

strace截取和记录系统进程调用，以及进程收到的信号。是一个非常有效的检测、指导和调试工具。系统管理员可以通过该命令容易地解决程序问题。
使用该命令需要指明进程的ID(PID)，例如：
strace \-p <pid>
\# strace –p 2582
rt\_sigprocmask(SIG\_SETMASK, \[\], NULL, 8) = 0
read(7, "\\"\\\\\\"\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"..., 16384) = 321
write(3, "}H\\331q\\37\\275$\\271\\t\\311M\\304$\\317~)R9\\330Oj\\304\\257\\327"..., 360) = 360
select(8, \[3 4 7\], \[3\], NULL, NULL)     = 2 (in \[7\], out \[3\])
rt\_sigprocmask(SIG\_BLOCK, \[CHLD\], \[\], 8) = 0
rt\_sigprocmask(SIG\_SETMASK, \[\], NULL, 8) = 0
read(7, "\\"\\\\\\"\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"..., 16384) = 323
write(3, "\\204\\303\\27$\\35\\206\\\\\\306VL\\370\\5R\\200\\226\\2\\320^\\253\\253"..., 360) = 360
select(8, \[3 4 7\], \[3\], NULL, NULL)     = 2 (in \[7\], out \[3\])
rt\_sigprocmask(SIG\_BLOCK, \[CHLD\], \[\], 8) = 0
rt\_sigprocmask(SIG\_SETMASK, \[\], NULL, 8) = 0
read(7, "\\"\\\\\\"\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"..., 16384) = 323
write(3, "\\243\\207\\204\\277Cw\\0162\\2ju=\\205\\'L\\352?0J\\256I\\376\\32"..., 360) = 360
select(8, \[3 4 7\], \[3\], NULL, NULL)     = 2 (in \[7\], out \[3\])
rt\_sigprocmask(SIG\_BLOCK, \[CHLD\], \[\], 8) = 0
rt\_sigprocmask(SIG\_SETMASK, \[\], NULL, 8) = 0
read(7, "\\"\\\\\\"\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\"..., 16384) = 320
write(3, "6\\270S\\3i\\310\\334\\301\\253!ys\\324\\'\\234%\\356\\305\\26\\233"..., 360) = 360
select(8, \[3 4 7\], \[3\], NULL, NULL)     = 2 (in \[7\], out \[3\])
rt\_sigprocmask(SIG\_BLOCK, \[CHLD\], \[\], 8) = 0
rt\_sigprocmask(SIG\_SETMASK, \[\], NULL, 8) = 0
**
12****、****ulimit**

ulimit内置在bash shell中，用来提供对shell和进程可用资源的控制
liyawei:~ # ulimit \-a
core file size          (blocks, \-c) 0
data seg size           (kbytes, \-d) unlimited
file size               (blocks, \-f) unlimited
pending signals                 (\-i) 6143
max locked memory       (kbytes, \-l) 32
max memory size         (kbytes, \-m) unlimited
open files                      (\-n) 1024
pipe size            (512 bytes, \-p) 8
POSIX message queues     (bytes, \-q) 819200
stack size              (kbytes, \-s) 8192
cpu time               (seconds, \-t) unlimited
max user processes              (\-u) 6143
virtual memory          (kbytes, \-v) unlimited
file locks                      (\-x) unlimited
\-H和\-S选项指明所给资源的软硬限制。如果超过了软限制，系统管理员会收到警告信息。硬限制指在用户收到超过文件句炳限制的错误信息之前，可以达到的最大值。
例如可以设置对文件句炳的硬限制：ulimit \-Hn 4096
例如可以设置对文件句炳的软限制：ulimit \-Sn 1024
查看软硬值，执行如下命令：
ulimit \-Hn
ulimit \-Sn
例如限制Oracle用户. 在/etc/security/limits.conf输入以下行:
soft nofile 4096
hard nofile 10240
对于Red Hat Enterprise Linux AS，确定文件/etc/pam.d/system\-auth包含如下行
session required /lib/security/$ISA/pam\_limits.so
对于SUSE LINUX Enterprise Server，确定文件/etc/pam.d/login 和/etc/pam.d/sshd包含如下行：
session required pam\_limits.so
这一行使这些限制生效。
**
13****、****mpstat**

mpstat是Sysstat工具集的一部分，下载地址是[http://perso.wanadoo.fr/sebastien.godard/](http://perso.wanadoo.fr/sebastien.godard/)
mpstat用于报告多路CPU主机的每颗CPU活动情况，以及整个主机的CPU情况。
例如，下边的命令可以隔2秒报告一次处理器的活动情况，执行3次
mpstat 2 3
liyawei:~ # mpstat 2 3
Linux 2.6.16.21\-0.8\-default (liyawei)   05/31/07

10:23:03     CPU   %user   %nice    %sys %iowait    %irq   %soft  %steal   %idle    intr/s
10:23:05     all    0.50    0.00    0.00    1.99    0.00    0.00    0.00   97.51    271.64
10:23:07     all    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    261.00
10:23:09     all    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    261.50
Average:     all    0.17    0.00    0.00    0.67    0.00    0.00    0.00   99.17    264.73
如下命令每隔1秒显示一次多路CPU主机的处理器活动情况，执行3次
mpstat \-P ALL 1 3
liyawei:~ # mpstat \-P ALL 1 10
Linux 2.6.16.21\-0.8\-default (liyawei)   05/31/07

10:23:31     CPU   %user   %nice    %sys %iowait    %irq   %soft  %steal   %idle    intr/s
10:23:32     all    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    273.00
10:23:32       0    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    272.00
10:23:33     all    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    254.00
10:23:33       0    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    254.00
10:23:34     all    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    271.00
10:23:34       0    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00    271.00
10:23:35     all    0.00    0.00    0.00    1.98    0.00    0.00    0.00   98.02    254.46
10:23:35       0    0.00    0.00    0.00    1.98    0.00    0.00    0.00

98.02    254.46


[CentOS性能监控工具_好习惯_新浪博客](http://blog.sina.com.cn/s/blog_531bb763010144xr.html)