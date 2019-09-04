---
tags: [GitHub]
title: Home > linux下删除乱码文件
created: '2019-09-04T08:11:26.830Z'
modified: '2019-09-04T08:11:36.891Z'
---

[Home](http://linux48.com/index.html) > [linux下删除乱码文件](http://linux48.com/2015-01-06-linux-rm-luanma.html)

# linux下删除乱码文件

作者：[Chris](http://linux48.com)

根据inode 来修改或删除linux 下乱码的文件

1.  创建测试文件：
    touch 1?.txt

2.  查询inode ：

```
[oracle@test]$ ll -i
total 14694452
17956913 -rw-r--r-- 1 oracle oinstall          0 Jan 18 20:24 1?.txt

```

3.  修改测试文件名：

```
find . -inum 17956913 -exec mv {} file.txt \;

```

4.  检查修改结果

```
[oracle@test]$ ll
total 14694452
....
-rw-r--r-- 1 oracle oinstall          0 Jan 18 20:24 file.txt

```

记录：删除乱码的文件可使用

```
find . -inum 17956913 -exec rm {} \;
```

[linux下删除乱码文件 - linux48](http://linux48.com/2015-01-06-linux-rm-luanma.html)

