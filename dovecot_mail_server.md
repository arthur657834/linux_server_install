```
yum -y install postfix dovecot  
yum -y install cyrus-sasl-*

vi /etc/postfix/main.cf  


vi /etc/dovecot/dovecot.conf  


vi /etc/dovecot/conf.d/10-auth.conf  

vi /etc/dovecot/conf.d/10-mail.conf


vi /etc/dovecot/conf.d/10-ssl.conf 

vi /etc/sysconfig/saslauthd

vi /usr/lib64/sas12/smtpd.conf 

systemctl  start  dovecot  
systemctl  start  postfix  
systemctl  start  saslauthd 


空白主机安装
http://www.ewomail.com/


```
