nginx的root 指令
转载 2013年02月21日 00:27:47 26666
好长时间都没搞清nginx的root路径：

location /img/ {
    alias /var/www/image/;
}
#若按照上述配置的话，则访问/img/目录里面的文件时，ningx会自动去/var/www/image/目录找文件
location /img/ {
    root /var/www/image;
}
#若按照这种配置的话，则访问/img/目录下的文件时，nginx会去/var/www/image/img/目录下找文件。] 

alias是一个目录别名的定义，root则是最上层目录的定义。

一直以为root是指的/var/www/image目录下，应该 是 /var/www/image/img/ 

还有一个重要的区别是alias后面必须要用“/”结束，否则会找不到文件的。。。而root则可有可无~~