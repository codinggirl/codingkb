# PostgreSQL重置密码

#### 编辑pg\_hba.conf文件

sudo vi /Library/PostgreSQL/9.4/data/pg\_hba.conf

在文件近末尾处，修改local的METHOD由"md5"改为"trust"：

\# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
#local   all             all                                     md5       #修改前
local   all             all                                     trust      #修改后

#### 找到PostgreSQL的服务名

执行以下命令：

ls /Library/LaunchDaemons

PostgreSQL的服务名为：

com.edb.launchd.postgresql\-9.4.plist

#### 重启PostgreSQL服务

执行以下命令停止服务：

sudo launchctl stop com.edb.launchd.postgresql\-9.4.plist

执行以下命令启动服务：

sudo launchctl start com.edb.launchd.postgresql\-9.4.plist

或在执行：su postgres命令切换到postgres用户下执行以下命令重启：

/Library/PostgreSQL/9.4/bin/pg\_ctl restart \-D /Library/PostgreSQL/9.4/data

#### 启动Postgre会话

执行以下命令：

```
psql \-U postgres
```

#### 重置密码

在psql会话中执行如下命令修改密码：

ALTER USER postgres WITH PASSWORD '新密码';

操作完成的，执行：\\q命令回车退出。

#### 恢复pg\_hba.conf设置为md5并重启服务

