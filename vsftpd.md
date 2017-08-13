```
安装vsftpd
1、以管理员（root）身份执行以下命令
yum -y install vsftpd
2、设置开机启动vsftpd ftp服务
chkconfig vsftpd on
3、启动vsftpd服务
service vsftpd start
管理vsftpd相关命令：
停止vsftpd:  service vsftpd stop
重启vsftpd:  service vsftpd restart
配置vsftpd服务器
默认的配置文件是/etc/vsftpd/vsftpd.conf，你可以用文本编辑器打开。
vi /etc/vsftpd/vsftpd.conf
添加ftp用户
下面是添加ftpuser用户，设置根目录为/home/wwwroot/ftpuser,禁止此用户登录SSH的权限，并限制其访问其它目录。
１、修改/etc/vsftpd/vsftpd.conf
将底下三行
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
改为
chroot_list_enable=YES
# (default follows)
chroot_list_file=/etc/vsftpd/chroot_list
3、增加用户ftpuser，指向目录/home/wwwroot/ftpuser,禁止登录SSH权限。
useradd -d /home/ftpuser -g ftp -s /sbin/nologin ftpuser
4、设置用户口令
passwd ftpuser
5、编辑文件chroot_list:
vi /etc/vsftpd/chroot_list
内容为ftp用户名,每个用户占一行,如：
peter
john
6、重新启动vsftpd
 
cannot change directory:/home/***
ftp服务器连接失败,错误提示:
500 OOPS: cannot change directory:/home/*******
500 OOPS: child died
解决方法:
在终端输入命令：
setsebool -P ftpd_disable_trans 1
service vsftpd restart
就ＯＫ了！
原因：这是因为服务器开启了selinux，这限制了FTP的登录。
 
临时关闭selinux    setenforce 0

永久关闭：

修改/etc/selinux/config 文件



将SELINUX=enforcing改为SELINUX=disabled

查看SELinux状态：



1、/usr/sbin/sestatus -v      ##如果SELinux status参数为enabled即为开启状态



SELinux status:                 enabled



2、getenforce                 ##也可以用这个命令检查


 
修改iptables规则：
1.vi /etc/sysconfig/iptables-config
加入以下两行
#add by lj
IPTABLES_MODULES="ip_conntrack_ftp"
IPTABLES_MODULES="ip_nat_ftp"
2 .vi /etc/sysconfig/iptables
#newadd lj
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --sport 21 -j ACCEPT
3.  service iptables restart


windows通过我的电脑打开vsftp会出现pub文件夹，原因是vsftp允许匿名登录，修改配置文件不允许就可以了
anonymous_enable=NO 


500 OOPS: vsftpd: cannot locate user specified in 'ftp_username':ftp
这个错误很诡异的在CentOS和台机的ubuntu上没有出现，但在本上却发生了。在/etc/vsftpd.conf中添加一行ftp_username=nobody就搞定



root用户登录

[root@localhost ~]# vi /etc/vsftpd/ftpusers   #注销root

[root@localhost ~]# vi /etc/vsftpd/user_list   #注销root

[root@localhost ~]# service vsftpd restart



500 OOPS: vsftpd: refusing to run with writable root inside chroot ()  

配置文件中加

allow_writeable_chroot=YES


```
