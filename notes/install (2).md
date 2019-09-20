nginx安装与反向代理
2014-05-19
想要在服务器上搭配N多站点，使用反向代理，监听80端口是个好方法，并且Nginx的效率很高。

Window
下载地址.选择nginx/Windows-1.7.0下载。

解压缩到本地，这里我选择D:\Servers下，解压之后完整路径为D:\Servers\nginx-1.7.0。

在D:\Servers\nginx-1.7.0下,启动使用start nginx。

是否启动可以通过tasklist | grep nginx查看。记住，不要多次启动，不然后台会挂很多nginx的。

Linux
sudo apt-get install nginx安装即可。安装完毕默认启动。

设置反向代理
这里举个例子，就是通过本机ip访问的转到3000端口，localhost访问的转到4000端口。下面看具体设置。

Windows
配置文件目录在D:\Servers\nginx-1.7.0\conf下， 修改nginx.conf文件，去除其中的server配置，添加上include vhosts/*.conf;。

在D:\Servers\nginx-1.7.0\conf下创建vhosts目录，里面方式各个server配置即可。

这里给一个我的配置（使用localhost访问4000端口， 192.168.1.60访问3000端口）的例子：

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
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
upstream nodejs__upstream {
     server 127.0.0.1:3000;
     keepalive 64;
}
server {
     listen 80;
     server_name 192.168.1.60;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream;
     }
}
upstream nodejs__upstream2 {
     server 127.0.0.1:4000;
     keepalive 64;
}
server {
     listen 80;
     server_name localhost;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream2;
     }
}
Linux
配置文件目录在/etc/nginx中。

Linux下的配置文件结构和Windows略有不同，但是意义相似。修改/etc/nginx/nginx.conf文件：

1
2
3
4
5
6
7
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
# 修改为
include /etc/nginx/conf.d/*.conf;
# include /etc/nginx/sites-enabled/*;
include /etc/nginx/vhosts/*.conf;
注意这里，我新建了个目录vhosts，如果不想新建，就把里面的配置文件放到conf.d或者sites-enabled中。

vhosts目录下我包含了两个文件，default.conf和proxy-node.conf。

default.conf
此文件就是sites-enabled下的defalut文件。

proxy-node.conf
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
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
upstream nodejs__upstream {
     server 127.0.0.1:3000;
     keepalive 64;
}
server {
     listen 80;
     server_name 192.168.1.60;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream;
     }
}
upstream nodejs__upstream2 {
     server 127.0.0.1:4000;
     keepalive 64;
}
server {
     listen 80;
     server_name localhost;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream2;
     }
}
保存完毕之后，重启nginx。 sudo nginx -s reload。

之后通过localhost和192.168.1.60访问就能对应分别的端口了。

这里说下node做服务器时listen的设置，只绑定端口，无需绑定ip。如果像官网上写的:

1
2
3
4
5
6
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
那么只能通过localhost:1337和127.0.0.1:1337访问。无法通过192.168.1.60:1337这样访问。



[nginx安装与反向代理](http://finalhome.org/%E6%9C%8D%E5%8A%A1%E5%99%A8/nginx%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86/)
