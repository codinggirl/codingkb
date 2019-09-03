# RAID

RAID, Reduandant Array of Indenpent Disk，独立冗余磁盘阵列。

最初由加州大学伯克利分校提出，其目的是用小的廉价磁盘来代替大的昂贵磁盘，同时希望磁盘损坏时不会对数据造损失。

## RAID 工作模式

常见的有 4 种工作模式：RAID0、RAID1、RAID5、RAID10。

## RAID0

数据分条技术。

容错性：无
冗余性：无
读性能：高
连续写性能：高
随机写性能：高
需要磁盘：2个或2个以上
可用容量：总的磁盘容量
典型应用：无故障的快速读写，安全性要求不高，如图形工作站

## RAID1

磁盘镜像。

磁盘利用率低，存储成本高。
数据安全性最好。

## RAID5

容错性：有
冗余类型：奇偶校验
需要磁盘数：3个或3个以上
典型应用：安全性高的场景，如银行、数据库、存储

## RAID10

镜像阵列条带。

数据跨磁盘抽取，每个磁盘都有一个镜像阵列。

需要 4 + 2 * N (N >= 0) 个磁盘存储器。