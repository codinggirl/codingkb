nginx 多站点配置方法集合
转载  2011-06-28   作者：     我要评论

关于nginx的多站设置，其实和apache很相似，假设我们已经有两个域名，分别是：www.websuitA.com和www.websuitB.com。并且这两个域名已经映射给了IP为192.168.1.1的服务器。
那么我们开始吧： 
1、为我们的站点创建配置文件 
　　我是这么做的，在nginx的配置文件conf目录下创建一个专门存放VirtualHost的目录，命名为vhosts_conf，可以把虚拟目录的配置全部放在这里。在里面创建名为vhosts_modoupi_websuitA.conf的配置文件并打开，我们在这里做配置，往里面写： 
复制代码 代码如下:

server { 
listen 80;　　　　　　　　　　　　　　　#监听的端口号 
server_name websuitA.com;　　　　　　　　#域名 
#access_log logs/host.access.log main; 
location / { 
root X:/wnmp/www/websuitA;　　　　#站点的路径 
index default.php index.php index.html index.htm; 
#站点的rewrite在这里写 
rewrite ^/(\w+)\.html$ /$1.php; 
rewrite ^/(\w+)/(\w+)$ /$1/$2.php; 
} 
#错误页的配置 
error_page 404 /error.html; 
error_page 500 502 503 504 /50x.html; 
location = /50x.html { 
root html; 
} 
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000 
location ~ \.php$ { 
root X:/wnmp/www/websuitA; 
fastcgi_pass 127.0.0.1:9000; 
fastcgi_index index.php; 
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name; 
include fastcgi_params; 
} 
location ~ /\.ht { 
deny all; 
} 
} 

这样就做好了，站点A的配置，同样的方法，做websuitB的配置，这里我命名为vhosts_modoupi_websuitB.conf，直接上代码 
复制代码 代码如下:

server { 
　　　　 listen 80;　　　　　　　　　　　　　　　#监听的端口号 
　　　　 server_name websuitB.com;　　　　　　　　#域名 
　　　　 #access_log logs/host.access.log main; 
　　　　 location / { 
　　　　 　　 root X:/wnmp/www/websuitB;　　　　#站点的路径 
　　　　　　 index default.php index.php index.html index.htm; 
#站点的rewrite在这里写 
　　　　　　　rewrite ^/(\w+)\.html$ /$1.php; 
　　　　　　　rewrite ^/(\w+)/(\w+)$ /$1/$2.php; 
　　　　　} 
　 #错误页的配置 
　　　　　error_page 404 /error.html; 
　　　　　error_page 500 502 503 504 /50x.html; 
　　　　　location = /50x.html { 
　　　　　　　root html; 
　　　　　} 
　　　　 # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000 
　　　　 location ~ \.php$ { 
　　　　　　　　root X:/wnmp/www/websuitB; 
　　　　　　　　fastcgi_pass 127.0.0.1:9000; 
　　　　　　　　fastcgi_index index.php; 
　　　　　　　　fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name; 
　　　　　　　　include fastcgi_params; 
　　　　　} 
　　　　　location ~ /\.ht { 
　　　　　　　　deny all; 
　　　　　} 
} 

这样，两个站点的配置就OK了。 
2、在nginx的主配置文件里，包含这两个站点的配置文件。 
　　我们打开conf目录下的nginx.conf文件，很容易做，只要在http{...}段输入以下代码： 
复制代码 代码如下:

#包含所有的虚拟主机的配置文件 
include X:/wnmp/nginx/conf/vhosts_conf/*.conf; 

这样，nginx的多站点配置就做好了，怎么样打开浏览器测试一下吧~ 

第二种方法：
当我们有了一个 VPS 主机以后，为了不浪费 VPS 的强大资源（相比共享主机1000多个站点挤在一台机器上），往往有想让 VPS 做点什么的想法，银子不能白花啊:)。放置多个网站或者博客是个不错的想法，可是如何配置 web 服务器才能在一个 VPS 上放置多个网站/博客呢？如何通过一个 IP 访问多个站点/域名呢？这就是大多数 web 服务器支持的 virtual hosting 功能。这里将描述如何一步一步如何用 nginx 配置 virtual hosting。 
nginx 是一个小巧高效的 web 服务器，由俄罗斯程序员 Igor Sysoev 开发，nginx 虽然体积小，但功能一点也不弱，能和其他的 web 服务器一样支持 virtual hosting，即一个IP对应多个域名以支持多站点访问，就像一个IP对应一个站点一样，所以是”虚拟”的。你想在一个 IP 下面放多少个站点就放多少，只要硬盘够大就行。 
这里以配置2个站点（2个域名）为例，n 个站点可以相应增加调整，假设： 
IP地址: 202.55.1.100 
域名1 example1.com 放在 /www/example1 
域名2 example2.com 放在 /www/example2 
配置 nginx virtual hosting 的基本思路和步骤如下： 
把2个站点 example1.com, example2.com 放到 nginx 可以访问的目录 /www/ 
给每个站点分别创建一个 nginx 配置文件 example1.com.conf，example2.com.conf, 并把配置文件放到 /etc/nginx/vhosts/ 
然后在 /etc/nginx.conf 里面加一句 include 把步骤2创建的配置文件全部包含进来（用 * 号） 
重启 nginx 
具体过程 
下面是具体的配置过程： 
1、在 /etc/nginx 下创建 vhosts 目录 
1 
mkdir /etc/nginx/vhosts 
2、在 /etc/nginx/vhosts/ 里创建一个名字为 example1.com.conf 的文件，把以下内容拷进去 
复制代码 代码如下:

server { 
listen 80; 
server_name example1.com www. example1.com; 
access_log /www/access_ example1.log main; 
location / { 
root /www/example1.com; 
index index.php index.html index.htm; 
} 
error_page 500 502 503 504 /50x.html; 
location = /50x.html { 
root /usr/share/nginx/html; 
} 
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000 
location ~ \.php$ { 
fastcgi_pass 127.0.0.1:9000; 
fastcgi_index index.php; 
fastcgi_param SCRIPT_FILENAME /www/example1.com/$fastcgi_script_name; 
include fastcgi_params; 
} 
location ~ /\.ht { 
deny all; 
} 
} 

3、在 /etc/nginx/vhosts/ 里创建一个名字为 example2.com.conf 的文件，把以下内容拷进去 
复制代码 代码如下:

server { 
listen 80; 
server_name example2.com www. example2.com; 
access_log /www/access_ example1.log main; 
location / { 
root /www/example2.com; 
index index.php index.html index.htm; 
} 
error_page 500 502 503 504 /50x.html; 
location = /50x.html { 
root /usr/share/nginx/html; 
} 
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000 
location ~ \.php$ { 
fastcgi_pass 127.0.0.1:9000; 
fastcgi_index index.php; 
fastcgi_param SCRIPT_FILENAME /www/example2.com/$fastcgi_script_name; 
include fastcgi_params; 
} 
location ~ /\.ht { 
deny all; 
} 
} 

4、打开 /etc/nginix.conf 文件，在相应位置加入 include 把以上2个文件包含进来 
复制代码 代码如下:

user nginx; 
worker_processes 1; 
# main server error log 
error_log /var/log/nginx/error.log ; 
pid /var/run/nginx.pid; 
events { 
worker_connections 1024; 
} 
# main server config 
http { 
include mime.types; 
default_type application/octet-stream; 
log_format main '$remote_addr - $remote_user [$time_local] $request ' 
'"$status" $body_bytes_sent "$http_referer" ' 
'"$http_user_agent" "$http_x_forwarded_for"'; 
sendfile on; 
#tcp_nopush on; 
#keepalive_timeout 0; 
keepalive_timeout 65; 
gzip on; 
server { 
listen 80; 
server_name _; 
access_log /var/log/nginx/access.log main; 
server_name_in_redirect off; 
location / { 
root /usr/share/nginx/html; 
index index.html; 
} 
} 
# 包含所有的虚拟主机的配置文件 
include /usr/local/etc/nginx/vhosts/*; 
} 

5、重启 Nginx 
第三种方法:
一个服务器上需要跑多个网站，如果仅仅把域名解析到server的IP是不行的，访问不同域名打开的都是nginx默认的网站。要想分别对应，需要在nginx里设置vhost。 

我是用lnmp一键安装包(http://www.lnmp.org/ )安装的nginx+mysql+php环境，对于其他自己编译的nginx估计配置文件和安装目录会有所不同，自己酌情修改哦，呵呵 
编辑/usr/local/nginx/conf/nginx.conf，去掉server的参数。 
复制代码 代码如下:

server 
{ 
listen 80; 
server_name www.wifizoo.net; 
index index.html index.htm index.php; 
root /tmp/wwwroot; 本文来自 
location ~ .*\.(php|php5)?$ 
{ 
fastcgi_pass unix:/tmp/php-cgi.sock; 
fastcgi_index index.php; 
include fcgi.conf; 
} copyright 
location /status { 
stub_status on; 
access_log off; 
} 
copyright 
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ 
{ 
expires 30d; 
} 

location ~ .*\.(js|css)?$ 
{ 
expires 12h; 
} 

log_format access '$remote_addr - $remote_user [$time_local] "$request" ' 
'$status $body_bytes_sent "$http_referer" ' 
'"$http_user_agent" $http_x_forwarded_for'; 
access_log /home/wwwroot/logs/access.log access; 
} 

然后加入vhost定义： copyright 
include /usr/local/nginx/vhost/*.conf; 
} 
再在/usr/local/nginx/建立vhost文件夹，里面创建各域名的对应配置文件。 
这个简单，就把之前的server配置内容复制到创建的对应conf文件里就OK了。 
复制代码 代码如下:

server 
{ 
listen 80; 
server_name www.jb51.net; 
server_name jb51.net; 
index index.html index.htm index.php; 
root /tmp/wwwroot/meituge; 

location ~ .*\.(php|php5)?$ 
{ 
fastcgi_pass unix:/tmp/php-cgi.sock; 
fastcgi_index index.php; 
include fcgi.conf; 
} copyright 
location /status { 
stub_status on; 
access_log off; 
} 
copyright 

location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ 
{ 
expires 30d; 
} 
copyright 

location ~ .*\.(js|css)?$ 
{ 
expires 12h; 
} 
#log_format access '$remote_addr - $remote_user [$time_local] "$request" ' 
#'$status $body_bytes_sent "$http_referer" ' 
#'"$http_user_agent" $http_x_forwarded_for'; 
#access_log /home/wwwroot/logs/access.log access; 
} 

这里要注意，如果你用的是一级域名，那么需要在server配置里指定不加www前缀的域名，否则访问jb51.net会被定义到默认站点而非www.jb51.net 
server_name www.jb51.net; 
server_name jb51.net;