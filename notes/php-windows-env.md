5
NOV/13
在windows上以php-fpm的形式部署nginx服务器
In case anyone still needs this info. This is how I setup php-fpm for use with nginx on windows:

Download the .zip file from http://windows.php.net/download/. The .zip file should be VC9 which has the FastCGI file (php-cgi.exe). Don't download VC6, and don't download the .msi file because it requires that you have IIS setup already in order to install php-fpm. The zip file contains the php-cgi.exe which is what you need for php-fpm. I downloaded a slightly older version, php-5.3.10-Win32-VC9-x86.zip, from here http://windows.php.net/downloads/releases/archives/ because I wanted to match the version running on my production server.

Unzip the file, e.g. unzip into C:\php-5.3.10-Win32-VC9-x86

Edit the php.ini file as needed. What I did:

# nginx security setting
cgi.fix_pathinfo=0

extension_dir = "C:\php-5.3.10-Win32-VC9-x86\ext"
enable the following modules by uncommenting them:

extension=php_curl.dll
extension=php_mbstring.dll
extension=php_mysqli.dll
Create a .bat file somewhere, e.g. start-php-fcgi.bat in nginx directory or in the php directory:

@ECHO OFF
ECHO Starting PHP FastCGI...
set PATH=C:\php-5.3.10-Win32-VC9-x86;%PATH%
C:\php-5.3.10-Win32-VC9-x86\php-cgi.exe -b 127.0.0.1:9123 -c C:\php-5.3.10-Win32-VC9-x86\php.ini
Double click the .bat file to start php-fpm. A window will popup and stay open while its running. Its kind of annoying, but just haven't looked into setting it up as service yet.

In the php section of nginx.conf I reference the 127.0.0.1:9123:

location ~ \.php$ {
fastcgi_pass 127.0.0.1:9123;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}








 Windows下配置Nginx和PHP7
标签： php
2016-03-03 09:22 6001人阅读 评论(0) 收藏 举报
 分类： PHP（30）  
版权声明：本文为博主原创文章，未经博主允许不得转载。

目录(?)[+]

准备
下载 Nginx Windows版并解压； 
下载 PHP7 Windows版 并解压。

配置 Nginx
双击nginx.exe会闪退，但是在任务管理器中会发现其实已经启动了nginx.exe *32，据说这是正常的。 
在浏览器中输入localhost可以看到页面就说明启动成功了。

配置文件

配置文件是conf/nginx.conf。可以修改端口号、根目录等。

配置网站根目录

location / {
    root   ../www;
    index  index.html index.htm index.php;
}
允许列出目录

http {
    include       mime.types;
    default_type  application/octet-stream;

    autoindex on; #允许列出目录
    ...
开启错误日志

找到

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
把前面的#去掉即可。

配置PHP FastCGI server

找到

#location ~ \.php$ {
#    root           html;
#    fastcgi_pass   127.0.0.1:9000;
#    fastcgi_index  index.php;
#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
#    include        fastcgi_params;
#}
把前面的#去掉并修改fastcgi_param SCRIPT_FILENAME

location ~ \.php$ {
    root           ../www;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  D:\www$fastcgi_script_name;
    include        fastcgi_params;
}
意思是，所有以 php 为后缀的文件都交给127.0.0.1:9000来处理，而这里的 IP 地址和端口就是 FastCGI 进程监听的 IP 地址和端口。

需要注意的是D:\www$fastcgi_script_name;中的D:\www不能再用相对路径../www了。

现在还不能访问 PHP 页面，需要开启 PHP CGI。

配置PHP7
复制一份php.ini-development改为php.ini，搜索cgi.fix_pathinfo=1，把前面的分号去掉，保存。

启动php-cgi.exe，如果弹出如下错误

这里写图片描述

即使修复了一个，还会有另一个，所以，干脆下载安装Visual C++，如果是 Win7 则需要是 Win7 SP1

这里写图片描述

安装好Visual C++ Redistributable for Visual Studio 2015之后，在命令行运行php-cgi.exe

php-cgi.exe -b 127.0.0.1:9000
测试
写一个测试

// test.php
<?php
    pathinfo();
打浏览器中打开localhost:8099/test.php

















windows下配置nginx+php环境
　　刚看到nginx这个词，我很好奇它的读法（engine x），我的直译是“引擎x”，一般引“擎代”表了性能，而“x”大多出现是表示“xtras（额外的效果）”，那么整个词的意思就是类似“极致效果”，“额外性能”。当然这里不是要来唠嗑，以上是题外话。

　　nginx相较于我们熟悉的apache、IIS的优势，就我浅入浅出的了解，在于“反向代理”和“负载均衡”。因此考虑到能够为Web服务器节省资源，它可以代替apache来提供Web服务。那么上正题了，nginx有这么多优势，那在windows下如何来配置nginx+php环境？网上看到还是那么多转载来转载去的文章。这里就我配置的过程，来介绍一下：

1、首先需要准备的应用程序包。

　　nginx：nginx/Windows-1.0.4

　　php：php-5.2.16-nts-Win32-VC6-x86.zip （nginx下php是以FastCGI的方式运行，所以我们下载非线程安全也就是nts的php包）

　　（还会用到）RunHiddenConsole：RunHiddenConsole.zip

2、安装与配置。

　1）php的安装与配置。

　　直接解压下载好的php包，到D盘wnmp目录（D:\wnmp），这里把解压出来的文件夹重命名成php5。进入文件夹修改php.ini-recommended文件为php.ini，并用Editplus或者Notepad++打开来。找到

extension_dir = "./ext"
更改为

extension_dir = "D:/wnmp/php5/ext"
往下看，再找到
;extension=php_mysql.dll
;extension=php_mysqli.dll
前面指定了php的ext路径后，只要把需要的扩展包前面所对应的“;”去掉，就可以了。这里打开php_mysql.dll和php_mysqli.dll，让php支持mysql。当然不要忘掉很重要的一步就是，把php5目录下的libmysql.dll文件复制到C:\Windows目录下，也可以在系统变量里面指定路径，当然这里我选择了更为方便的方法^_^。

到这里，php已经可以支持mysql了。

　　接下来我们来配置php，让php能够与nginx结合。找到


;cgi.fix_pathinfo=1
我们去掉这里的封号。

cgi.fix_pathinfo=1
这一步非常重要，这里是php的CGI的设置。
　2）nginx的安装与配置。

　　把下载好的nginx-1.0.4的包同样解压到D盘的wnmp目录下，并重命名为nginx。接下来，我们来配置nginx，让它能够和php协同工作。进入nginx的conf目录，打开nginx的配置文件nginx.conf，找到

location / {
      root   html;　　　　　　#这里是站点的根目录
      index  index.html index.htm;
}
将root  html;改为root   D:/wnmp/www;

再往下，找到


复制代码
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
#location ~ \.php$ {
#    root           html;
#    fastcgi_pass   127.0.0.1:9000;
#    fastcgi_index  index.php;
#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
#    include        fastcgi_params;
#}
复制代码
先将前面的“#”去掉，同样将root  html;改为root   D:/wnmp/www;。再把标记为红色的/scripts改为“$document_root”，这里的“$document_root”就是指前面“root”所指的站点路径，这是改完后的：

复制代码
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
location ~ \.php$ {
      root           D:/wnmp/www;
      fastcgi_pass   127.0.0.1:9000;
      fastcgi_index  index.php;
      fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
      include        fastcgi_params;
}
复制代码
保存配置文件，就可以了。

　　nginx+php的环境就初步配置好了，来跑跑看。我们可以输入命令 

来启动php，并手动启动nginx，当然也可以利用脚本来实现。

　　首先把下载好的RunHiddenConsole.zip包解压到nginx目录内，RunHiddenConsole.exe的作用是在执行完命令行脚本后可以自动关闭脚本，而从脚本中开启的进程不被关闭。然后来创建脚本，命名为“start_nginx.bat”，我们在Notepad++里来编辑它

复制代码
@echo off
REM Windows 下无效
REM set PHP_FCGI_CHILDREN=5

REM 每个进程处理的最大请求数，或设置为 Windows 环境变量
set PHP_FCGI_MAX_REQUESTS=1000
 
echo Starting PHP FastCGI...
RunHiddenConsole D:/wnmp/php5/php-cgi.exe -b 127.0.0.1:9000 -c D:/wnmp/php5/php.ini
 
echo Starting nginx...
RunHiddenConsole D:/wnmp/nginx/nginx.exe -p D:/wnmp/nginx
复制代码
再另外创建一个名为stop_nginx.bat的脚本用来关闭nginx

复制代码
@echo off
echo Stopping nginx...  
taskkill /F /IM nginx.exe > nul
echo Stopping PHP FastCGI...
taskkill /F /IM php-cgi.exe > nul
exit
复制代码
做好后，是这样的



这样，我们的服务脚本也都创建完毕了。双击start_nginx.bat看看进程管理器是不是有两个nginx.exe的进程和一个php-cgi.exe的进程呢？



这样nginx服务就启动了，而且php也以fastCGI的方式运行了。

到站点目录下，新建一个phpinfo.php的文件，在里面编辑

<?php
    phpinfo();
?>
保存后，打开浏览器输入“http://localhost/phpinfo.php”，如果看到



就说明，nginx+php的环境已经配置好了，呵呵~























环境：Windows10

　　　mysql-5.6.24-win32解压缩版

　　   nginx-1.8.0

　　   php7

1、Mysql安装

　　下载压缩文件之后解压缩至相应目录（我的目录是G:\wnmp\mysql-5.6.24-win32）

　　mysql根目录下修改my-default.ini文件为my.ini文件：修改将basedir和datadir两个路径前面的#号去掉改为

　　　　basedir = G:\wnmp\mysql-5.6.24-win32
　　　　datadir = G:\wnmp\mysql-5.6.24-win32\data（目录替换为相应的解压路径）

　   以管理员身份运行cmd，

　　G：

　　cd \wnmp\mysql-5.6.24-win32

　　mysqld -install

　　net start mysql

　　服务启动成功，如果报错1067说明配置文件有问题

2、Php安装

　  php7下载下来解压之后进入Php7目录下执行php-cgi -b 9000 -q即可（端口对应下面nginx配置）

3、nginx安装

　  修改nginx配置文件nginx.conf

　　location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

　　启动nginx即可

到这里初步环境就配置完成，可以新建php文件在浏览器里面测试是否安装成功！

关于phpinfo下的Configuration File (php.ini) Path问题：

我这里打印的是C:\Windows下 所以特意查了下相关问题，得到下面的结果：

WINDOWS环境下的PHP运行时首先在C:\WINDOWS里找配置文件，找不到就在PHP安装目录下找，两个目录都找不到就使用PHP默认的配置环境。所以你只要在C:\WINDOWS或PHP安装目录下至少有一个适当的PHP配置文件就可以了。

4、php -r "file_put_contents('symfony', file_get_contents('https://symfony.com/installer'));" 使用php命令生成文件报错

　　原因：没有加载php_openssl.dll这个扩展

　　打开配置文件：   extension=php_openssl.dll

　　　　　　　　　　allow_url_include = On 修改这两次地方重启php-cgi运行还是报错，扩展加载路径不对修改

　　　　　　　　　　extension_dir = "ext"

　　接着又报错SSL 握手协议超时。。。未解决！















windows下将任意程序（PHP）添加为服务
原创 2016年12月11日 18:56:55 标签：php /web server /windows /srvany /instsrv 1324
题记
本文主要阐述instsrv.exe 和 srvany.exe 将某程序（PHP）添加到windows 服务中。 
近期遇到一些情况，web服务如果按照Apache+PHP+MySql 搭建起来代价有点高。Mysql可以用Sqlite代替。PHP有自带的Web Server。这样一个PHP就可以解决。略微麻烦的是后台执行和开机启动的问题。

如何使用PHP自带Web Server
命令 php -S {server}:{port} [-t path] [router.php] [-file webServer.php] 
例句 php -S lcoalhost:80 -t D:\path router.php-file webServer.php

{server} 必填 域名、主机名或IP地址 
{port} 必填 监听的端口号 
[-t path] 选填 如果不填默认为当前目录 
[router.php] 选填 不填将不使用路由 
[-file webServer.php] 选填 不填将不修改webServer机制

路由文件
如果路由为 index.php

$url = parse_url($_SERVER['REQUEST_URI']);
$uri = $url['path'];
if ($uri != "/" && $uri != "/index.php" && file_exists(dirname(__FILE__).$uri)) {
    //请求的文件或目录存在，直接返回
    return false; 
}
1
2
3
4
5
6
如果路由不为 index.php

$url = parse_url($_SERVER['REQUEST_URI']);
$uri = $url['path'];
if (file_exists(dirname(__FILE__).$uri)) {
    //请求的文件或目录存在，直接返回
    return false; 
}
1
2
3
4
5
6
以上代码 能实现类似于 以下 .htaccess 文件的效果

<IfModule mod_rewrite.c>
  Options +FollowSymlinks
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
1
2
3
4
5
6
7
服务器配置文件
webServer.php文件 复制自php官方，请自行脑补或测试其功能

<?php
if (preg_match('/\.css|\.js|\.jpg|\.png|\.map$/', $_SERVER['REQUEST_URI'], $match)) {
    $mimeTypes = [
        '.css' => 'text/css',
        '.js'  => 'application/javascript',
        '.jpg' => 'image/jpg',
        '.png' => 'image/png',
        '.map' => 'application/json'
    ];
    $path = __DIR__ . $_SERVER['REQUEST_URI'];
    if (is_file($path)) {
        header("Content-Type: {$mimeTypes[$match[0]]}");
        require $path;
        exit;
    }
}
require_once __DIR__.'/../app/bootstrap.php';
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
创建 windows服务
安装与删除
instsrv.exe 与 srvany.exe的资料网上较少，基本都是相互复制。 
至今没找到64位版的程序，不过我在64位Win10测试无异常 
instsrv.exe 的作用是将 srvany.exe 添加到windows 服务中 
srvany.exe 的作用是通过服务方式加载指定程序。 
instsrv.exe 与 srvany.exe可以放到任意目录 并非必须放在 System32

添加 C:\server\instsrv.exe {Name} C:\server\srvany.exe 
删除 C:\server\instsrv.exe {Name} remove

添加服务后打开注册表 regedit
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\{Name} 

服务名称项目右击新建项，名称为Parameters。
定位到Parameters项，新建以下几个字符串值。
名称 Application 值为你要作为服务运行的程序地址。
名称 AppDirectory 值为你要作为服务运行的程序所在文件夹路径。
名称 AppParameters 值为你要作为服务运行的程序启动所需要的参数。
注册表配置

通过服务 services.msc 管理
这里写图片描述

找到添加的服务 {Name} 可以与普通服务一样管理

后记
将PHP直接添加到服务中的意义，不仅在于省掉一个Apache之类的东西。还可以在Windows环境中使用PHP创建其他常驻内存的服务(如果对性能要求不高的话)。


























1. 安装Apache

下载地址：http://www.apache.org/dyn/closer.cgi/httpd/binaries/win32

如需更改端口：打开Apache安装目录下conf目录下的httpd.conf文件，找到Listen 80，80即为现在所用端口，更改80为其他端口即可；

2. 安装php

下载地址：http://windows.php.net/download （下载有php5apache2_2.dll文件的版本）

如下载php-5.2.17-Win32-VC6-x86.zip文件（VC9是专门为IIS定制的，VC6 是为了其他WEB服务软件提供的，如 Apache），解压后放于C盘（或其他位置），则路径为C:\php；

3.  PHP环境配置

将php.ini-dist或php.ini-development配置文件重命名为php.ini（该文件中分号开始行为注释内容），然后做如下设置：

　　1） extension_dir = "C:/php5/ext"

　　2） 取消下列设置的注释：

　　extension=php_curl.dll
　　extension=php_gd2.dll
　　extension=php_mbstring.dll
　　extension=php_mysql.dll
　　extension=php_pdo_mysql.dll
　　extension=php_pdo_odbc.dll
　　extension=php_xmlrpc.dll

3） 配置Session功能：

      在使用session功能时，必须配置session文件在服务器上的保存目录，否则无法使用session，在Windows 7上新建一个可读写的目录文件夹，此目录最好独立于WEB主程序目录之外，在D盘根目录上建立了phpsessiontmp目录，然后在php.ini配 置文件中找到 ;session.save_path = "/tmp"  改成  session.save_path = "D:/phpsessiontmp"

4） 配置PHP的文件上传功能

      同session一样，在使用PHP文件上传功能时，我们必须要指定一个临时文件夹以完成文件上传功能，否则文件上传功能会失败，我们仍然需要在 Windows 7上建立一个可读写的目录文件夹，此处我在D盘根目录上建立了phpfileuploadtmp目录，然后在php.ini配置文件中找到 ;upload_tmp_dir = 改成 upload_tmp_dir = "D:/phpfileuploadtmp"

5） 修改date.timezone

;date.timezone = 改成 date.timezone = Asia/Shanghai

4.  配置Apache以支持PHP

1） 在#LoadModule vhost_alias_module modules/mod_vhost_alias.so下添加

LoadModule php5_module "c:/php/php5apache2_2.dll"
PHPIniDir "c:/php"
AddType application/x-httpd-php .php .html .htm

注：在PHP目录下可以看到多个php5apache的DLL文件，由于使用的是Apache2.2，所以需要使用php5apache2_2.dll，接着指定PHP的安装目录以及执行的程序扩展名。

2） 道默认Apache服务器执行WEB主程序的目录为Apache2.2/htdocs，所以当你的WEB主程序目录变更时，我们需要修改相应的Apache配置，即将

DocumentRoot "C:/Program Files/Apache Software Foundation/Apache2.2/htdocs"

改为

DocumentRoot "D:/PHPWeb"   //D:/PHPWeb为自己站点目录

将

<Directory "C:/Program Files/Apache Software Foundation/Apache2.2/htdocs">

改为

<Directory "D:/PHPWeb">

3） 最后修改具体的index文件先后顺序，由于配置了PHP功能，当然需要index.php优先执行；

DirectoryIndex index.html

改为

DirectoryIndex index.php index.html

4） 重启Apache服务器

在Apache服务器上PHP环境配置工作就完成了，只需要在D:/PHPWeb目录下新建一个PHP文件，写入

<?
phpinfo();
?>

然后在游览器中输入http://localhost:88/index.php，就可以看到PHP的具体配置页面。

5.  安装MySQL

下载地址：http://www.mysql.com/downloads

6.  IDE可以是用Zend Studio

http://www.geekso.com/ZendStudio9-key

 

REF:

http://www.cnblogs.com/Peter-Zhang/archive/2012/12/10/2810915.html

http://www.cnblogs.com/pharen/archive/2012/02/06/2340628.html

http://windows.php.net/download/









Win+Nginx+PHP+MySQL环境搭建
1. Win+Nginx+PHP+MySQL环境搭建

Nginx是一个高性能的HTTP和反向代理服务器，同时还是IMAP/POP3/SMTP代理服务器，该程序由俄罗斯Rambler.ru 站点开发，Nginx因为性能稳定、低系统资源消耗而闻名。近几年Nginx在国内已经成炙热化状态，比如像腾讯、网易、51CTO、迅雷、当当网、 51、人人网等诸多大型网站都已经使用Nginx来做Web服务器，所以学习Nginx还是非常有必要的。2011年6月1日，nginx 1.0.4发布。

1.1. 程序下载

PHP下载：进入PHP官网下载网址：http://windows.php.net/download/。选择最新版本“VC9 x86 Thread Safe (2011-Mar-22 13:27:32)” (如图1)，弹出保存对话框，保存在硬盘目录下(如图2)。



图1



图2

Non Thread Safe：非安全线程。Thread Safe与之相反。

Zip：压缩包。

Installer：安装包。

Debug Pack：调试包。

MySQL下载：进入MySQL官网下载网址：http://dev.mysql.com/downloads/mirror.php?id=402887。选择“No thanks,just take me to the downloads!” (如图3)。选择最新镜像“mysql-installer-5.5.13.0.msi” (如图4)，弹出保存对话框，保存在硬盘目录下(如图5)。



图3



图4



图5

Nginx下载：进入Nginx官网下载网址：http://nginx.org/en/download.html。选择最新版本 “nginx/Windows-1.0.4 pgp” (如图6)，弹出保存对话框，保存在硬盘目录下(如图7)。



图6



图7

1.2. 程序安装

在E盘新建目录为DedeCMS，把程序全部安装在此文件夹中。

PHP配置：在DedeCMS目录下新建文件夹PHP5，压缩包解压至此目录，把php.ini-production重命名为php.ini，用编辑器打开此文件(最好不要用记事本打开)，此文件为PHP配置文件。

查找定位至：; extension_dir = "./" ，将前面的分号去掉并修改为：extension_dir = "E:DedeCMSPHP5ext"

查找定位至：;extension=php_gd2.dll，将前面的分号去掉为：extension=php_gd2.dll

查找定位至：;extension=php_mbstring.dll，将前面的分号去掉为：extension=php_mbstring.dll

查找定位至：;extension=php_mysql.dll，将前面的分号去掉为：extension=php_mysql.dll

查找定位至：;extension=php_mysqli.dll，将前面的分号去掉为：extension=php_mysqli.dll

查找定位至：;extension=php_pdo_mysql.dll，将前面的分号去掉为：extension=php_pdo_mysql.dll

查找定位至：;cgi.force_redirect = 1，将前面的分号去掉为：cgi.force_redirect = 1

查找定位至：;cgi.fix_pathinfo=1，将前面的分号去掉为：cgi.fix_pathinfo=1

查找定位至：;cgi.rfc2616_headers = 0，将前面的分号去掉为：cgi.rfc2616_headers = 1

MySQL安装：双击安装包进入安装环境界面，选择“Install MySQL Products”(如图8)。



图8

选择“I accept the license terms”，再选择“Next”按钮继续安装(如图9)。



图9

选择“Skip the check for updates(not recommanded)”，再选择“Next”按钮继续安装(如图10)。



图10

选择“Custom”，更改安装路径为之前创建的目录。再选择“Next”继续安装(如图11)。



图11

选择“MySQL Server 5.5.13”与“MySQL Workbench 5.2.34”， “MySQL Workbench 5.2.34”是可视数据库设计工具。这个工具是设计MySql数据库的专用工具。也可根据需要选择是否安装。选择“Next”按钮继续安装(如图 12)。



图12

选择“Next”按钮继续安装(如图13)。



图13

稍等片刻。程序安装完成，选择“Next”按钮进行产品配置(如图14)。



图14

选择“Server Machine”，再选择“Next”按钮继续(如图15)。



图15

默认3306端口，默认MySQL55 Windows服务进程名称。输入密码与重复密码。选择“Next”按钮继续(如图16)。



图16

程序会自动安装，安装完成选择“Next”出现安装完成界面(如图17)。



图17

Nginx配置：将压缩包nginx-1.0.4目录下的所有文件夹解压至DedeCMS目录，打开conf目录下nginx.conf配置文件。

第45行改为：index index.html index.htm index.php; 加了PHP文件支持。

第65至71行改为：

location ~ .php$ {

root html;

fastcgi_pass 127.0.0.1:9000;

fastcgi_index index.php;

fastcgi_param ｓｃｒｉｐｔ_FILENAME e:/dedecms/html$fastcgi_ｓｃｒｉｐｔ_name;

include fastcgi_params;

}

e:/dedecms/html为网站的根目录。此目录也可以重新配置。找到代码：

location / {
root html;
index index.html index.htm index.php;
}

其中 root html; 就是网站根目录，如可以改为：root wwwroot;

1.3. 服务的启动与关闭
建立两个文件分别启动、关闭服务进程。

在DedeCMS中建立文件start_nginx.bat

@echo off

echo Starting PHP FastCGI...

RunHiddenConsole e:/DedeCMS/PHP5/php-cgi.exe -b 127.0.0.1:9000 -c e:/DedeCMS/PHP5/php.ini

echo Starting nginx...

RunHiddenConsole e:/DedeCMS/nginx.exe

Exit

在DedeCMS中建立文件stop_nginx.bat

@echo off

echo Stopping nginx...

taskkill /F /IM nginx.exe > nul

echo Stopping PHP FastCGI...

taskkill /F /IM php-cgi.exe > nul

exit

下载软件：http://www.haoboweb.com/uploads/allimg/110613/RunHiddenConsole.zip。(只能上传图片，传不了软件)

解压至DedeCMS目录，双击start_nginx.bat启动服务进程（如图18）。由此可见，stop_nginx.bat 文件为关闭服务进程。



图18

1.4. 环境测试
在网站根目录中建立index.php文件，内容为：

<html>

<head>

<title>Welcome to nginx!</title>

</head>

<body>

<?php

$link = mysql_connect("localhost","root","123456");

if($link){

echo "数据库连接成功！";

}else{

echo "数据库连接失败！";

}

?>

</body>

</html>

输出“数据库连接成功！”，nginx的PHP环境配置成功（如图19）。



图19











