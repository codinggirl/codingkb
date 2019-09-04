---
title: BigTable
created: '2019-09-04T02:47:49.811Z'
modified: '2019-09-04T02:49:22.601Z'
---

# BigTable

bigtable是一个用来存储结构数据的分布式存储系统。与平时常用的数据库不同，bigtable并非一个支持sql语言的关系数据库，而是map方式的，列导向的数据库（一列数据连续存储）。bigtable为读进行了优化，对数据库的读取访问远远大于写入是互联网服务的重要特点。bigtable的时间特性也颇为引人注目，bigtable中数据都带有timestamp字段，可以保存不同时间的多个版本。

论文中提到，google已经有6个服务已经运行于bigtable上了。分别是：Google Analytics,Google Earth,Personalized Search,Google Finance, Orkut,Writely。

特别值得注意的是，这6个服务都是恰好带有明显时间特性的服务，借助bigtable的时间特性，可谓如虎添翼。最近Google Earth也增加了时间的标签。将来，bigtable必将用于更多的地方，事实上，时间标签对于web服务是相当重要的特征，但由于数据量太大，保存困难，限制了很多应用的发展，bigtable应用于wiki或是archive.org之类的服务的时候，必将势如破竹。

[google发布bigtable论文 - HuoJu's BLOG](https://jhuo.ca/post/googlebigtable/)

