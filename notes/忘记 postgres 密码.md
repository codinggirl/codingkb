# 忘记 Postgres 密码

如果忘记了 Postgres 密码，可以通过以下步骤来重置密码。

## 修改 pg_hba.conf 文件

### 找到 pg_hba.conf 文件的路径

运行

```shell
ps ax | grep postgres | grep -v postgres:
```

```shell
  591 ?        S      1:57 /usr/lib/postgresql/11/bin/postgres -D /var/lib/postgresql/11/main -c config_file=/etc/postgresql/11/main/postgresql.conf
11333 pts/0    S+     0:00 grep postgres
```

我们看到有`config_file`的一项, `config_file=/etc/postgresql/11/main/postgresql.conf`，进入配置文件所在的目录，可以看到 pg_hba.config 文件。

### 修改文件，配置无密码登录

修改 pg\_hba.confg，将：

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
```

改成

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
```

这里更改了 IPv4 的情况，如果你使用的是 Unix domain socket 或 IPv6，根据实际情况修改。

### 重启postgresql服务

```
sudo service postgresql restart
```

## 修改数据库密码

```
psql -h 127.0.0.1 -U postgres
```

```
alter user postgres with password 'YOUR　PASSWORD';
```

## 重新配置 pg_hba.conf，重启 Postgres，以新密码登录

将 pg_hba.conf 配置为之前的配置，当然也可不配置。

重启 Postgres 服务。

```
sudo service postgresql restart
```

使用新的密码来操作数据库。

```
psql -h 127.0.0.1 -U postgres
```
