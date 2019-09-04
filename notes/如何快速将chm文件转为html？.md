---
tags: [GitHub]
title: 如何快速将chm文件转为html？
created: '2019-09-04T07:57:57.563Z'
modified: '2019-09-04T07:58:07.798Z'
---

# 如何快速将chm文件转为html？

发布于 2013\-06\-29  3.91k 次阅读

---

CHM文件是Windows的一种帮助文件，它主要是由.html转换制作而来的，因其可以在Windows系统下快速打开、操作便捷、形式多样的特性，也常被采用作为电子书的格式。

我们常见的CHM文件是通过系统中的hh.exe程序来执行的，其实这个程序不仅可以将CHM文件打开，还具有将chm转为html的命令。

转换语法为：
hh \-decompile 目标文件夹 源CHM文件名。
如：hh \-decompile d:\\test\\help help.chm

为了让操作更为简单，也可使用批处理执行。

1、新建一个文本文件，文本内容如下：

> @echo off
> title CHM转HTML反编器
> color a
> echo.
> set /p urlfile=请将需要转换的CHM文件移进来，然后按回车键：
> copy %urlfile% chmfile.chm > nul
> hh \-decompile .\\CHM chmfile.chm
> rem del /q chmfile.chm > nul 可以将这句话前面的rem去掉，去掉后反编译成功后则删除chm源文件。
> echo.
> echo 反编文件成功。保存在.\\CHM文件夹中，按任意键退出！
> start "" explorer http://jishukong.net
> exit

2、然后把文本文件的扩展名txt改为bat，以管理员身份运行此文件；
3、打开后，将CHM文件拖入窗口中，按回车键执行反编译；
4、一般数秒钟后（视chm源文件大小而定），文件转换成功。

\[newauthor title='软件推荐'\]当然也可使用第三方工具对CHM文件进行转化，如chm2web、CHM Encoder，只是觉得没有这个必要；如果需要将HTML文件转为CHM文件，推荐使用Easy CHM。\[/newauthor\]


[如何快速将chm文件转为html？ – 技术控](http://jishukong.net/287.html)


