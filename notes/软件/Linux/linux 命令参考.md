# Unix/Linux 命令参考

## 文件命令

## 进程管理

## 文件权限

## SSH

## 搜索

## 系统信息

## 压缩

## 网络

## 安装

## 快捷键

文件命 令
系统信 息
ls – 列出目录
date – 显示当前日期和时间
ls -al – 使用格式化列出隐藏文件
cal – 显示当月的日历
cd dir - 更改目录到 dir
uptime – 显示系统从开机到现在所运行的时间
cd – 更改到 home 目录
w – 显示登录的用户
pwd – 显示当前目录
whoami – 查看你的当前用户名
mkdir dir – 创建目录 dir
finger user – 显示 user 的相关信息
rm file – 删除 file
uname -a – 显示内核信息
rm -r dir – 删除目录 dir
cat /proc/cpuinfo – 查看 cpu 信息
rm -f file – 强制删除 file
cat /proc/meminfo – 查看内存信息
rm -rf dir – 强制删除目录 dir *
man command – 显示 command 的说明手册
cp file1 file2 – 将 file1 复制到 file2
df – 显示磁盘占用情况
cp -r dir1 dir2 – 将 dir1 复制到 dir2; 如果 dir2 不存 在则创建它
du – 显示目录空间占用情况
free – 显示内存及交换区占用情况
mv file1 file2 – 将 file1 重命名或移动到 file2; 如果 file2 是一个存在的目录则将 file1 移动到目录 file2 中
压缩
ln -s file link – 创建 file 的符号连接 link
tar cf file.tar files – 创建包含 files 的 tar 文件 file.tar
touch file – 创建 file
tar xf file.tar – 从 file.tar 提取文件
cat > file – 将标准输入添加到 file
tar czf file.tar.gz files – 使用 Gzip 压缩创建 tar 文件
more file – 查看 file 的内容
head file – 查看 file 的前 10 行
tar xzf file.tar.gz – 使用 Gzip 提取 tar 文件
tail file – 查看 file 的后 10 行
tar cjf file.tar.bz2 – 使用 Bzip2 压缩创建 tar 文 件
tail -f file – 从后 10 行开始查看 file 的内容
进程管 理
tar xjf file.tar.bz2 – 使用 Bzip2 提取 tar 文件
ps – 显示当前的活动进程
gzip file – 压缩 file 并重命名为 file.gz
top – 显示所有正在运行的进程
gzip -d file.gz – 将 file.gz 解压缩为 file
kill pid – 杀掉进程 id pid
网络
killall proc – 杀掉所有名为 proc 的进程 *
ping host – ping host 并输出结果
bg – 列出已停止或后台的作业
whois domain – 获取 domain 的 whois 信息
fg – 将最近的作业带到前台
dig domain – 获取 domain 的 DNS 信息
fg n – 将作业 n 带到前台
dig -x host – 逆向查询 host
文件权 限
wget file – 下载 file
chmod octal file – 更改 file 的权限 ● 4–读(r)
● 2–写(w)
● 1–执行(x) 示例:
chmod 777 – 为所有用户添加读、写、执行权限
chmod 755 – 为所有者添加 rwx 权限, 为组和其他用户添加 rx 权限
更多选项参阅 man chmod.
wget -c file – 断点续传
安装
从源代码安装: ./configure make
make install
dpkg -i pkg.deb – 安装包 (Debian)
rpm -Uvh pkg.rpm – 安装包 (RPM)
快捷键
SSH
Ctrl+C – 停止当前命令
ssh user@host – 以 user 用户身份连接到 host
Ctrl+Z – 停止当前命令，并使用 fg 恢复
ssh -p port user@host – 在端口 port 以 user 用户身 份连接到 host
Ctrl+D – 注销当前会话，与 exit 相似
Ctrl+W – 删除当前行中的字
ssh-copy-id user@host – 将密钥添加到 host 以实现无 密码登录
Ctrl+U – 删除整行
!! - 重复上次的命令
exit – 注销当前会话 * 小心使用。
翻译/Toy <http://LinuxTOY.org>
 
搜索
grep pattern files – 搜索 files 中匹配 pattern 的内容
grep -r pattern dir – 递归搜索 dir 中匹配 pattern 的 内容
command | grep pattern – 搜索 command 输出中匹配 pattern 的内容

