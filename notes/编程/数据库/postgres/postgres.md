# Postgres


## 数据库的导出与导入

导出

```sh
# 导出
pg_dump --username <username> -f <sql file.sql> <db name>
```

导出过程中无需输入用户名。

导入

```
# 导入
psql -h 127.0.0.1 -U <username> -d <db name> -f <file name.sql>
```

命令解释：

`psql` — Postgres 命令行程序。
`-d <db name>` — 连接到数据库 <db name>。
`-f <sql file.sql>` — 运行文件中的 SQL 语句。

实际参数根据情况进行增减。

# Postgres 备份及恢复、数据导入与导出

有价值的数据需要经常备份。在此之前，我们需要了解备份及恢复的手段。

在 Postgres 中，有 3 种基本的数据备份方法：

- SQL 转储
- 文件系统级别的备份
- 持续归档

在 Postgres 中，导入与导出主要使用 pg_dump、pg_dumpall、pg_restore、psql 等命令。需要注意的是，数据库版本不同，所用的命令参数可能不同。

## 导出思路

- 导出全局对象
- 导出公共对象
- 导出数据库特定对象、数据库结构
- 导出数据

## 导入思路

- 在目标实例上建立全局对象
- 导入数据库特定对象、数据库结构
- 导入数据

## pg_dump 可以导出的格式

- SQL 文件

适合数据集比较小的情况。

可以通过 psql 进行数据恢复，或通过数据库管理工具进行。
将文件中的 SQL 语句执行一边，速度较慢。对于大量数据很难处理。

- 目录

适合大数据集。

可以使用 pg_dump 进行并发导出。
可以使用 pg_restore 进行并发导入。
每个表一个文件。
有数据压缩。

- 自定义

适合中小规模数据集。

输出的是单个文件，数据量过大时不好处理。

使用 pg_restore 进行并发导入。

- 压缩格式

## 导出公共对象

```
pg_dumpall -h <host> -g -p <port> -f <file>
```

## 导出特定数据库上的对象及数据库结构

```
pg_dump -s -C -v -fd<> -h <host> -Udxm -p <port>
```

### pg_dump 选项定义

-a 导出数据，不包含数据库结构
-s 导出库中的所有对象，不导出数据
-C 将创建数据库语句输出到文件中
-O 移除分配数据库所有者的语句
-x 移除授权、取消授权的语句
-Fc 自定义格式
-Fd 目录格式
-j <num of thead> 以多线程方式导出，和 -Fd 配合使用
--help 更多选项参考

## 使用 pg_dump 导出

```
$ pg\_dump \-U postgres \-d pg \-f dump.sql
```

## 导入

```
$ createdb <db name>
$ psql -d <db name> -U <user> -f <sql file>
```
## 其他

- Postgres 还可以使用 copy 等方式进行数据的导入与导出

## 参考资料

[PostgreSQL: Documentation: 11: Chapter 25. Backup and Restore](https://www.postgresql.org/docs/11/static/backup.html)

[PostgreSQL: Documentation: 11: 25.1. SQL Dump](https://www.postgresql.org/docs/11/static/backup-dump.html)

[PostgreSQL: Documentation: 11: 25.2. File System Level Backup](https://www.postgresql.org/docs/11/static/backup-file.html)

[PostgreSQL: Documentation: 11: 25.3. Continuous Archiving and Point-in-Time Recovery (PITR)](https://www.postgresql.org/docs/11/static/continuous-archiving.html)


PostSQL 安全

## 认证安全

端口暴露
