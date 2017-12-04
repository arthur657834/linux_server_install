```
yum -y install nss-pam-ldapd pam_ldap
ll /usr/lib64/security/pam_ldap.so
 
cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd.bak$(date +%F)

cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak$(date +%F)

cat /etc/pam.d/vsftpd
auth       sufficient   pam_ldap.so  第一个auth
account    sufficient   pam_ldap.so  第一个account


[root@centos-us ~]# grep -v ^# /etc/nslcd.conf 
uid nslcd
gid ldap
binddn uid=ldapaccessor,ou=People,dc=xxxxx,dc=com
scope sub
uri ldap://10.1.2.x/
base ou=People,dc=xxxxxx,dc=com
bindpw xxxxxxx
filter passwd (objectClass=person)
filter shadow (objectClass=person)

[root@centos-us ~]# grep -v ^# /etc/vsftpd/vsftpd.conf
anonymous_enable=NO
local_enable=YES
write_enable=YES
download_enable=YES
local_umask=0333
anon_umask=0333
anon_upload_enable=YES
anon_mkdir_write_enable=YES
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_file=/var/log/xferlog
xferlog_std_format=YES
chroot_local_user=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
guest_enable=YES
guest_username=ftp
local_root=/opt/data
allow_writeable_chroot=YES
chown_uploads=YES
chown_username=ftp
log_ftp_protocol=YES
use_sendfile=YES
ftp_username=nobody
virtual_use_local_privs=YES
cmds_allowed=FEAT,REST,CWD,LIST,MDTM,MKD,NLST,PASS,PASV,PORT,PWD,QUIT,RMD,RNFR,RNTO,RETR,STOR,SIZE,TYPE,USER,ACCT,APPE,CDUP,HELP,MODE,NOOP,REIN,STAT,STOU,STRU,SYST

systemctl start vsftpd
systemctl start nslcd

nslcd调试:
nslcd --debug

```
