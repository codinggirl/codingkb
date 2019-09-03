nginx查看配置文件nginx.conf路径  
2013-10-18 10:25:54|  分类： nginx |举报|字号 订阅
    




  下载LOFTER我的照片书  |


当你执行 nginx -t 得时候，nginx会去测试你得配置文件得语法，并告诉你配置文件是否写得正确，同时也告诉了你配置文件得路径：
# nginx -t
nginx: the configuration file /usr/local/etc/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/etc/nginx/nginx.conf test is successful