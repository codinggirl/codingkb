Nginx作为反向代理服务器，多个不同项目共用80端口（转载）
转载 2016年03月22日 09:53:30 7499
实践Ok代码如下：


[plain] view plain copy
server {  
    listen 80;  
    server_name u.justwinit.cn;  
    location / {  
        proxy_pass http://127.0.0.1:9527;  
        proxy_redirect off;  
        proxy_set_header Host $host;  
        proxy_set_header X-Real-IP $remote_addr;  
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;                                                                                                                                   
    }  
}  

相信很多朋友已经遇到了这个问题，但是都没有什么好的解决方案。首先思路上觉得是只有一个公网IP，必须要有一个支持应用层的程序来进行转发，进行代理才能够顺利的把相应的请求发到相应的后端机器上，结果自然选择nginx来进行反向代理了。

环境：
宿主机是Windows2003，IP为10.1.1.3，装VMware Workstation 6.0。在windows2003上运行nginx的win版。VM开两台虚拟机，网络模式为NAT模式。一台IP为192.168.84.128，一个台为192.168.84.129。分别开启80端口进行web服务。
修改测试机的hosts文件，指定www.001.com和www.002.com到宿主机10.1.1.3。
目的：
通过对宿主机win2003上的nginx设置，使解析到宿主机IP上的域名能够正常访问后面的web服务。适用于只有一个公网IP需要部署多个虚拟机来提供80端口web服务，一个虚拟机一个IP对应一个域名。
方法：
1、在nginx.conf最后一个"}"前，加入"include proxy.conf;"。
2、在同目录下，建立"proxy.conf"文件，内容如下：


[plain] view plain copy
server {  
listen 80;  
server_name www.001.com;  
  location / {  
  proxy_pass http://192.168.84.129; //后端ip地址  
  proxy_redirect off; //关闭后端返回的header修改  
  proxy_set_header Host $host; //修改发送到后端的header的host  
  proxy_set_header X-Real-IP $remote_addr; //设置真实ip  
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
  }  
}  
  
server {  
listen 80;  
server_name www.002.com;  
  location / {  
  proxy_pass http://192.168.84.128; //后端ip地址  
  proxy_redirect off; //关闭后端返回的header修改  
  proxy_set_header Host $host; //修改发送到后端的header的host  
  proxy_set_header X-Real-IP $remote_addr; //设置真实ip  
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
  }  
}  




此番配置之后，已经可以用任何解析到宿主机的IP的域名，访问自己的站点了。
来源：
http://www.justwinit.cn/post/5828/

=================================================

说明：未对上面内容进行验证。