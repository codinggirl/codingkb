# 在 CentOS 下查看硬件信息

## 查看中央处理器信息

```
more /proc/cpuinfo | grep "model name"
```
```
grep "model name" /proc/cpuinfo
```
```
grep "CPU" /proc/cpuinfo
```

> model name	: Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz
> model name	: Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz

## 查看内存信息

```
grep MemTotal /proc/meminfo
```

> MemTotal:        8179548 kB

## 查看 CPU 位数

```
getconf LONG_BIT
```

> 64

## 查看当前linux的版本

```
more /etc/redhat-release
```
```
cat /etc/redhat-release
```

> cat: /etc/redhat-release: No such file or directory

## 查看内核版本
**uname \-r
uname \-a

**6、使用CentOS常用命令查看硬盘和分区
**df \-h fdisk \-l 也可以查看分区
du \-sh 可以看到全部占用的空间
du /etc \-sh 可以看到这个目录的大小
**7、使用CentOS常用命令查看安装的软件包
**查看系统安装的时候装的软件包
cat \-n /root/install.log
more /root/install.log | wc \-l

## 查看键盘布局

**cat /etc/sysconfig/keyboard
cat /etc/sysconfig/keyboard | grep KEYTABLE | cut \-f2 \-d=


## 查看 selinux 信息

**sestatus
sestatus | cut \-f2 \-d:
cat /etc/sysconfig/selinux
**10、使用CentOS常用命令查看ip，mac地址
**在ifcfg\-eth0 文件里你可以看到mac，网关等信息。 ifconfig cat /etc/sysconfig/network\-scripts/ifcfg\-eth0 | grep IPADDR cat /etc/sysconfig/network\-scripts/ifcfg\-eth0 | grep IPADDR | cut \-f2 \-d= ifconfig eth0 |grep “inet addr:” |awk ‘{print $2}’|cut \-c 6\- ifconfig | grep ‘inet addr:’| grep \-v ’127.0.0.1′ | cut \-d: \-f2 | awk ‘{ print $1}’ 查看网关 cat /etc/sysconfig/network 查看dns cat /etc/resolv.conf 十二：使用CentOS常用命令查
看默认语言
echo $LANG $LANGUAGE
cat /etc/sysconfig/i18n
**11、使用CentOS常用命令查看所属时区和是否使用UTC时间
**cat /etc/sysconfig/clock
**12、使用CentOS常用命令查看主机名
**cat /etc/sysconfig/network
修改主机名就是修改这个文件，同时最好也把host文件也修改。

## 查看开机运行时间

uptime
09:44:45 up 67 days, 23:32, …

## 查看主板信息
dmidecode |more

## 查看机器所有硬件信息

dmidecode |more

dmesg |more

这2个命令出来的信息都非常多,所以建议后面使用"|more"便于查看

**2.查看CPU信息**

   方法一:

   Linux下CPU相关的参数保存在 /proc/cpuinfo 文件里

   cat /proc/cpuinfo |more

   方法二:

   采用命令 dmesg | grep CPU 可以查看到相关CPU的启动信息

   查看CPU的位数:

   getconf LONG\_BIT

## 查看Mem信息

 cat /proc/meminfo |more （注意输出信息的最后一行:MachineMem:   41932272 kB）

 free \-m

 top

## 查看磁盘信息

   方法一:

   fdisk \-l 可以看到系统上的磁盘(包括U盘)的分区以及大小相关信息。

   方法二:

   直接查看

   cat /proc/partitions

## 查看网卡信息

   方法一：

   ethtool eth0 采用此命令可以查看到网卡相关的技术指标

   （不一定所有网卡都支持此命令）

   ethtool \-i eth1 加上 \-i 参数查看网卡驱动

   可以尝试其它参数查看网卡相关技术参数

   方法二：

   也可以通过dmesg | grep eth0 等看到网卡名字(厂家)等信息

   通过查看 /etc/sysconfig/network\-scripts/ifcfg\-eth0 可以看到当前的网卡配置包括IP、网关地址等信息。

   当然也可以通过ifconfig命令查看。

## 查看主板信息

 lspci

## 挂载ISO文件

mount \-o loop \-t iso9660 \*.iso mount\_point

卸载直接umount mount\_point即可

## 查看光盘信息

   方法一：

   插入CD光碟后，在本人的RHEL5系统里，光碟文件是 /dev/cdrom，

   因此只需 mount /dev/cdrom mount\_point 即可。

   \[root@miix tmp\]# mount /dev/cdrom mount\_point

   mount: block device /dev/cdrom is write\-protected, mounting read\-only

   其实仔细看一下，光驱的设备文件是 hdc

   \[root@miix tmp\]# ls \-l /dev/cdrom\*

   lrwxrwxrwx 1 root root 3 01\-08 08:54 /dev/cdrom \-> hdc

   lrwxrwxrwx 1 root root 3 01\-08 08:54 /dev/cdrom\-hdc \-> hdc

   因此我们也可以这样 mount /dev/hdc mount\_point

   如果光驱里没放入有效光盘，则报错：

   \[root@miix tmp\]# mount /dev/hdc mount\_point

   mount: 找不到介质

## 查看USB设备

   方法一：

   其实通过 fdisk \-l 命令可以查看到接入的U盘信息，本人的U盘信息如下：

   Disk /dev/sda: 2012 MB, 2012217344 bytes

   16 heads, 32 sectors/track, 7676 cylinders

   Units = cylinders of 512 \* 512 = 262144 bytes

       Device Boot       Start          End       Blocks   Id   System

   /dev/sda1    \*           16          7676     1961024    b   W95 FAT32

   U盘的设备文件是 /dev/sda，2G大小，FAT32格式。

   如果用户登陆的不是Linux图形界面，U盘不会自动挂载上来。

   此时可以通过手工挂载(mount)：

   mount /dev/sda1 mount\_point

   以上命令将U盘挂载到当前目录的 mount\_point 目录，注意挂的是 sda1 不是 sda。

   卸载命令是 umount mount\_point

   Linux默认没有自带支持NTFS格式磁盘的驱动，但对FAT32支持良好，挂载的时候一般不需要 \-t vfat 参数 。

   如果支持ntfs，对ntfs格式的磁盘分区应使用 \-t ntfs 参数。

   如果出现乱码情况，可以考虑用 \-o iocharset=字符集 参数。

   可以通过 lsusb 命令查看 USB 设备信息哦：

   \[root@miix tmp\]# lsusb

   Bus 001 Device 001: ID 0000:0000

   Bus 002 Device 001: ID 0000:0000

   Bus 003 Device 001: ID 0000:0000

   Bus 004 Device 002: ID 0951:1613 Kingston Technology

   Bus 004 Device 001: ID 0000:0000

\===================================================

                          获取内存，cpu真实核数方法

\===================================================

**linux****内存查看方式**

如下显示free是显示的当前内存的使用,\-m的意思是M字节来显示内容.我们来一起看看.

$ free \-m
              total       used       free     shared    buffers     cached
Mem:          1002        769        232          0         62        421
\-/+ buffers/cache:         286        715
Swap:          1153          0       1153

第一部分Mem行:
total  内存总数: 1002M
used  已经使用的内存数: 769M
free  空闲的内存数: 232M
shared  当前已经废弃不用，总是0
buffers Buffer 缓存内存数: 62M
cached Page 缓存内存数:421M

关系：total(1002M) = used(769M) + free(232M)

第二部分(\-/+ buffers/cache):
(\-buffers/cache) used内存数：286M (指的第一部分Mem行中的used \- buffers \- cached)
(+buffers/cache) free内存数: 715M (指的第一部分Mem行中的free + buffers + cached)

可见\-buffers/cache反映的是被程序实实在在吃掉的内存，而+buffers/cache反映的是可以挪用的内存总数。

第三部分是指交换分区,  我想不讲大家都明白.

我想大家看了上面,还是很晕.第一部分(Mem)与第二部分(\-/+ buffers/cache)的结果中有关used和free为什么这么奇怪.
其实我们可以从二个方面来解释.
对操作系统来讲是Mem的参数.buffers/cached  都是属于被使用,所以它认为free只有232.
对应用程序来讲是(\-/+ buffers/cach).buffers/cached  是等同可用的，因为buffer/cached是为了提高程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。

所以,以应用来看看,以(\-/+ buffers/cache)的free和used为主.所以我们看这个就好了.另外告诉大家一些常识.Linux为了提高磁盘和内存存取效率, Linux做了很多精心的设计,  除了对dentry进行缓存(用于VFS,加速文件路 径名到inode的转换),  还采取了两种主要Cache方式：Buffer Cache和Page Cache。前者针对磁盘块的读写，后者针对文件inode的读写。这些Cache能有效缩短了  I/O系统调用(比如read,write,getdents)的时间。

记住内存是拿来用的,不是拿来看的.不象windows,  无论你的真实物理内存有多少,他都要拿硬盘交换文件来读.这也就是windows为什么常常提示虚拟空间不足的原因.你们想想,多无聊,在内存还有大部分 的时候,拿出一部分硬盘空间来充当内存.硬盘怎么会快过内存.所以我们看linux,只要不用swap的交换空间,就不用担心自己的内存太少.如果常常  swap用很多,可能你就要考虑加物理内存了.这也是linux看内存是否够用的标准哦

## 查看CPU真实核数

Intel 有超线程技术(HT)，可以从逻辑上虚拟出一些 CPU 来。因此，我们需要查看 CPU Cores 来判断真实的核数。

```shell
cat /proc/cpuinfo
```

>   processor	: 0
    vendor_id	: GenuineIntel
    cpu family	: 6
    model		: 85
    model name	: Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz
    stepping	: 4
    microcode	: 0x1
    cpu MHz		: 2500.018
    cache size	: 33792 KB
    physical id	: 0
    siblings	: 2
    core id		: 0
    cpu cores	: 1
    apicid		: 0
    initial apicid	: 0
    fpu		: yes
    fpu_exception	: yes
    cpuid level	: 13
    wp		: yes
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single ibrs ibpb stibp kaiser fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx avx512f avx512dq rdseed adx smap avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1
    bugs		: cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf
    bogomips	: 5000.03
    clflush size	: 64
    cache_alignment	: 64
    address sizes	: 46 bits physical, 48 bits virtual
    power management:

    processor	: 1
    vendor_id	: GenuineIntel
    cpu family	: 6
    model		: 85
    model name	: Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz
    stepping	: 4
    microcode	: 0x1
    cpu MHz		: 2500.018
    cache size	: 33792 KB
    physical id	: 0
    siblings	: 2
    core id		: 0
    cpu cores	: 1
    apicid		: 1
    initial apicid	: 1
    fpu		: yes
    fpu_exception	: yes
    cpuid level	: 13
    wp		: yes
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single ibrs ibpb stibp kaiser fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx avx512f avx512dq rdseed adx smap avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1
    bugs		: cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf
    bogomips	: 5000.03
    clflush size	: 64
    cache_alignment	: 64
    address sizes	: 46 bits physical, 48 bits virtual
    power management:

对于物理 CPU 数量，可以通过不重复的 physical id 数量来判断。
