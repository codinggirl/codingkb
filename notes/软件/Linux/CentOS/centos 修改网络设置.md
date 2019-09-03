[CentOS 修改IP地址, DNS, 网关](http://www.21andy.com/new/20100227/1717.html)

# [CentOS 修改IP地址, DNS, 网关](http://www.21andy.com/new/20100227/1717.html "CentOS 修改IP地址, DNS, 网关")

时间：2010\-02\-27 16:26:15   类别： [技术](http://www.21andy.com/new/category/%e6%8a%80%e6%9c%af/ "技术")   访问：36228

**一、CentOS 修改IP地址**

修改对应网卡的IP地址的配置文件
**\# vi /etc/sysconfig/network\-scripts/ifcfg\-eth0**

修改以下内容

DEVICE=eth0 #描述网卡对应的设备别名，例如ifcfg\-eth0的文件中它为eth0
BOOTPROTO=static #设置网卡获得ip地址的方式，可能的选项为static，dhcp或bootp，分别对应静态指定的 ip地址，通过dhcp协议获得的ip地址，通过bootp协议获得的ip地址
BROADCAST=192.168.0.255 #对应的子网广播地址
HWADDR=00:07:E9:05:E8:B4 #对应的网卡物理地址
IPADDR=12.168.1.2 #如果设置网卡获得 ip地址的方式为静态指定，此字段就指定了网卡对应的ip地址
IPV6INIT=no
IPV6\_AUTOCONF=no
NETMASK=255.255.255.0 #网卡对应的网络掩码
NETWORK=192.168.1.0 #网卡对应的网络地址
ONBOOT=yes #系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备

**二、CentOS 修改网关**
修改对应网卡的网关的配置文件
\[root@centos\]# vi /etc/sysconfig/network

修改以下内容
NETWORKING=yes(表示系统是否使用网络，一般设置为yes。如果设为no，则不能使用网络，而且很多系统服务程序将无法启动)
HOSTNAME=centos(设置本机的主机名，这里设置的主机名要和/etc/hosts中设置的主机名对应)
GATEWAY=192.168.1.1(设置本机连接的网关的IP地址。例如，网关为10.0.0.2)

**三、CentOS 修改DNS**

修改对应网卡的DNS的配置文件
**\# vi /etc/resolv.conf**
修改以下内容

nameserver 8.8.8.8 #google域名服务器
nameserver 8.8.4.4 #google域名服务器

四、重新启动网络配置
**\# service network restart**
或
**\# /etc/init.d/network restart**

**修改 IP 地址**
即时生效:
**\# ifconfig eth0 192.168.0.2 netmask 255.255.255.0**
启动生效:
修改 **/etc/sysconfig/network\-scripts/ifcfg\-eth0**

**修改网关 Default Gateway**
即时生效:
**\# route add default gw 192.168.0.1 dev eth0**
启动生效:
修改 **/etc/sysconfig/network**

**修改 DNS**
修改 **/etc/resolv.conf**
修改后可即时生效，启动同样有效

**修改 host name**
即时生效:
**\# hostname centos1**
启动生效:
修改 **/etc/sysconfig/network**
