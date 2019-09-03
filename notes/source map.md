---
tags: [js]
title: source map
created: '2019-09-03T03:11:01.448Z'
modified: '2019-09-03T09:25:37.732Z'
---

# source map

## [](#简介 "简介")简介

Source Map 是前端开发中非常有用的一种文件，它提供了源码映射的机制，使开发人员能够在本地开发过程或者线上环境基于源码进行调试或者错误堆栈分析。

## [](#Source-Map-提案 "Source Map 提案")Source Map 提案

JavaScript、CSS 代码的 source map 是根据 [Source Map Revision 3 Proposal](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit#heading=h.1ce2c87bpj24)（这个文档在谷歌 Docs 里，如果打不开，可以[下载 PDF](https://borninsummer.com/files/2019/source_map_revision_3_proposal.pdf)）这个提案来实现的。提案里用一句话描述其特点为“Better bidirectional mapping”，即“更好的双向映射”。

![](https://borninsummer.com/images/2019/06/source-map.png)

按照提案一个 sourceMap 文件的内容实际上是一个 JSON 对象：
```
{
 "version" : 3,
 "file": "out.js",
 "sourceRoot": "",
 "sources": \["foo.js", "bar.js"\],
 "sourcesContent": \[null, null\],
 "names": \["src", "maps", "are", "fun"\],
 "mappings": "A,AAAB;;ABCDE;"
}
```

下面我们逐行介绍每个字段的含义。

**version**

生成当前 sourcemap 时所遵从的提案（Source Map Revision）版本号，是个正整数。

**file**

An optional name of the generated code that this source map is associated with.

**sourceRoot**

An optional source root, useful for relocating source files on a server or removing repeated values in the “sources” entry. This value is prepended to the individual entries in the “source” field.

**sources**

源文件列表，后面的 mappings 字段会用到这个列表。

**sourcesContent**

可选。源文件的内容。

**names**

A list of symbol names used by the “mappings” entry.

**mappings**

字符串，编码后的数据。

## [](#mappings-详解 "mappings 详解")mappings 详解

## [](#反解析示例 "反解析示例")反解析示例

## [](#References "References")References

1.  [vlq | github](https://github.com/Rich-Harris/vlq/tree/master/sourcemaps)
2.  [vlq | npm](https://www.npmjs.com/package/vlq)
3.  [vlq | wikipedia](https://en.wikipedia.org/wiki/Variable-length_quantity)
4.  [source map 的原理探究](https://www.cnblogs.com/Wayou/p/understanding_frontend_source_map.html)

