---
title: virtualenv的原理
created: '2019-09-05T01:22:43.877Z'
modified: '2019-09-05T01:22:45.769Z'
---

# [virtualenv的原理](https://www.kawabangga.com/posts/3543 "Permalink to virtualenv的原理")

Posted on [2019年6月14日](https://www.kawabangga.com/posts/3543 "上午12:17") by [laixintao](https://www.kawabangga.com/posts/author/laixintao "View all posts by laixintao") [6 Comments](https://www.kawabangga.com/posts/3543#comments)

每个语言都是有自己的包管理工具，包管理是一个又复杂又难的话题，我觉得复杂度跟GC这个话题相比都不为过了。有趣的是，每个语言选择的包管理方案、依赖解决方案都多多少少不太一样，或多或少每个语言都做出了一些选择和取舍。比如 Go 是直接依赖 Github 做依赖管理(被大家吐槽很多次)，node 是根据 package.json 下载到项目的 node\_modules 目录(被吐槽也很多，主要是下载的东西太多了，js 社区的一个风格又是依赖层层嵌套比较重)。Python 的包管理也被吐槽很多，大家吐槽的点主要是太复杂了吧，解决方案又太多，这方面很不Python哦。比如你要去了解 virtualenv,virtualenv wrapper,pyenv, pip, pipenv, 等等……

但是 Python 语言的机制决定了它解决依赖的原理是一样的，这篇文章就来解释 virtualenv 机制的原理。

首先我们来看一下，这些包管理机制要解决的最根本的问题是什么？是多个项目的版本冲突呀！比如A项目依赖了 x package 的版本1，B项目依赖了 y package 的版本2，两个版本又是不兼容的，怎么办？如果都装到系统目录下，那么肯定就有一个项目不可用了。virtualenv 就是来解决这个问题的。

一个实时是，Python 一开始并没有涉及 virtualenv 这样的机制来管理这个语言的包，而是因为 Python 自身寻找包的机制，导致了 virtualenv 这种包管理形式的出现。

## 从 sys.prefix 说起

当 Python 解释器启动的时候，它会从解释器所在的 Path 开始，加上 `/lib/python$VERSION/os.py` 来逐层向上查找。因为 `os.py` 是解释器启动强依赖的包。比如我现在的 Python 启动目录是 `/usr/bin/python3.7`，那么查找过程就是：

|

1

2

3

 |

/usr/bin/lib64/ python3.7/os.py

/usr/lib64/python3. 7/os.py

/lib64/python3.7/ os.py

 |

这里贴一下 `strace python` 的记录，以下是在我电脑上启动时候的真实查找 `os.py` 的过程：

|

1

2

3

4

 |

stat("/usr/bin/Modules/Setup",  0x7ffe68a7cb60 )  \=  \-1  ENOENT (No such file  or  directory)

stat("/usr/bin/lib64/python3.7/os.py",  0x7ffe68a77a40 )  \=  \-1  ENOENT (No such file  or  directory)

stat("/usr/bin/lib64/python3.7/os.pyc",  0x7ffe68a77a40 )  \=  \-1  ENOENT (No such file  or  directory)

stat("/usr/lib64/python3.7/os.py",  { st\_mode\=S\_IFREG|0644,  st\_size \=37756,  ...} )  \=  0

 |

找到之后会设置 `sys.prefix` 这个变量，解释器去找包的时候，就去 `prefix/lib/PythonX.Y` 中去找。virtualenv 的工作原理就是基于这个：如果你需要一个隔离的项目 virtualenv，那我就给你复制一个独立的 Python 解释器可执行文件，然后根据相对目录把你需要的包都放在这个解释器所在的目录下，这样这个解释器启动的时候就可以找到（并且只能找到）这个目录下的包，virtualenv 就实现了独立包依赖的方案。

## virtualenv 工作原理

virtualenv 是这样工作的：首先 virtualenv 会复制 Python 解释器的可执行文件到 $VENV\_PATH/bin/python，然后创建 `$VENV_PATH/lib/python3.7/xx.py` 到系统的 `os.py` 所在的目录的模块的软连接（这样可以节省空间）。根据我们上面说过的解释器的启动原理，启动的时候，根据解释器所在的目录，会找到 `VENV_PATH` 下面的包，我们安装包的时候，也是安装到这里。这个解释器所使用的包就和其他解释器隔离开了。

为什么解释器的可执行文件需要拷贝一份，而不是也通过软连接的方式呢？因为解释器会解析软连接的目标地址，如果使用软连接的话，包也会使用系统Python的。那硬链接可不可以呢？这个我没研究过，我看 virtualenv 的源代码里面有一个 `FIXME` [提出了这样的想法](https://github.com/pypa/virtualenv/blob/a4010617f5f234b76a6ed5cb4ceb481f21ce8cce/virtualenv.py#L1502)，但是没有去实践。

这就是它的基本工作原理了，使用的时候，无非就是将这个 virtualenv 的 bin 目录插入到 $PATH 的最前面。然后我们执行 pip Python 这样的命令，就会执行到 virtualenv 里面的。

## Python3

如果使用 Python3，那么在生产环境就不需要安装 virtualenv 来创建虚拟环境了，Python3 内置了 venv 模块。

直接使用 python3 \-m venv myenv 创建虚拟环境即可。

这个 venv 的原理，还是和上面我们说过的一样。但是 Python3 有一些提升，它的 Python 可执行文件是一个软连接了，用一个 pyvenv.cfg 来标志出 home 的位置。

|

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

 |

├──  myenv

│    ├──  bin

│    │    ├──  activate

│    │    ├──  activate.csh

│    │    ├──  activate.fish

│    │    ├──  easy\_install

│    │    ├──  easy\_install\-3.7

│    │    ├──  pip

│    │    ├──  pip3

│    │    ├──  pip3.7

│    │    ├──  python  \->  python3

│    │    └──  python3  \->  /usr/local/bin/python3

│    ├──  include

│    ├──  lib

│    │    └──  python3.7

│    └──  pyvenv.cfg

 |

它的文件内容如下：

|

1

2

3

4

 |

File:  myenv/pyvenv. cfg

home  \=  /usr/ local/bin

include\-system\-site\-packages \=  false

version  \=  3.7.3

 |

如果 include\-system\-site\-packages 为 true，解释器启动的时候就会将系统的库添加到 `sys.path` 里面，这样我们在虚拟环境就可以 import 系统中安装的包了。

参考资料：

1.  [https://realpython.com/python\-virtual\-environments\-a\-primer/#why\-the\-need\-for\-virtual\-environments](https://realpython.com/python-virtual-environments-a-primer/#why-the-need-for-virtual-environments)
2.  [https://rushter.com/blog/python\-virtualenv/](https://rushter.com/blog/python-virtualenv/)
