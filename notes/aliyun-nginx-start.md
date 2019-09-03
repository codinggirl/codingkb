阿里云安装nginx启动问题
时间  2016-09-19
栏目 阿里巴巴
原文   http://blog.csdn.net/u011698346/article/details/52586437
错误类型：Starting nginx: nginx: [emerg] socket() [::]:80 failed (97: Address family not supported by protocol)  

原因是配置文件中的端口监听错误，查看配置文件include在/etc/nginx/nginx.conf文件中，/etc/nginx/conf.d/default.conf中将下面这行注释掉




listen       [::]:80 default_server;