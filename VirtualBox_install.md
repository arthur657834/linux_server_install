```
cd /etc/yum.repos.d/
wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
yum -y install epel-release 
yum -y install kernel-devel dkms qt qt-x11 kernel-headers

yum list | grep -i VirtualBox
yum -y install VirtualBox-5.2
```
