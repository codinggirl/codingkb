---
title: "LaTex 学习笔记"
---

## 让表格中的单元格换行

自定义命令

```
\newcommand{\tabincell}[2]{\begin{tabular}@{}#1@#2\end{tabular}}
```

使用：

```
&\tabincell{c}{}& 
```

```
\begin{tabular}{|c|c|}
\hline
1 & thie first line\\
\hline
2 & \tabincell{c}{haha\\heihei\\zeze}\\
\hline
\end{tabular}
```

## 多行公式

```
\begin{equation}
\begin{split}
a &= b \\
c &= d
\end{split}
\end{equation}
```

每行只允许出现一个“&”，使用split命令后，编号上下居中显示。
每行公式不可以有空白行。


##

*   pdflatex 图片要用PDF格式的。
*   参考文献中需要全部大写的单词用{ }括起来。
*   ~产生一个不可伸长的空格。
*   生成pdf打印时选项：Page Scaling(页面比例)选择none(无)，否则打印出来的
    稿件小一圈，正反面的页眉线也无法对齐。
*   xelatex 需要改变definition中的字体定义。
*   {} 将阻止LATEX 吃掉命令后的所有空格: \\LaTeX{}
*   波浪线：$\\sim$ ;\\~
*   CJK\* 模式由于自动忽略中文字符之间的所有空格，因此没有上面的这种副作用。但是这也带来了另一方
    面的问题，就是如果想在中文字符中间加入空格就必须加以保护，避免被忽略。加保护的方法是在空格前面
    加上\\ 字符。实际上CJK\* 模式忽略中文字符后面的空格，因此中文后面如果接着英文，必须加上保护的空格 \\ 或者~ 符号，否则可能造成断行错误
*   注释掉一大段内容，不做编译？
    用命令
    \\iffalse
    和
    \\fi
*   《LATEX2 科技排版指南》
*   强制换行后可以加字号命令：
    \\TeX{} is pronounced as
    \\(\\tau\\epsilon\\chi\\).\\\\\[6pt\]
    100 m$^{3}$ of water\\\\\[6pt\]
    This comes from my
    \\begin{math}\\heartsuit\\end{math}
*   \\def是TeX命令，与\\newcommand等价，但不会检查是否已经有这条命令
    而且\\def还可以把参数放在模板里
    例如\\def\\ttt#1(#2){......}
    调用的时候就可以用\\ttt...(...)的格式了
*   \\ifx是一条判断语句，\\ifx#1#2 A \\else B \\fi 就是判断如果#1=#2的话，就做A否则做B
*   数字和括号前以及英文字符前加~
*   显示表示链接\\url
*   文件名不能有空格
*   citeup{}中，多个文献用逗号分隔，引用标记中不能有空格。
    

[(zz)latex 让表格中的单元格换行 | 哈工大空间服务](http://space.hit.edu.cn/hanshuai/post/24.html) 
[多行公式 | 哈工大空间服务](http://space.hit.edu.cn/hanshuai/post/21.html)