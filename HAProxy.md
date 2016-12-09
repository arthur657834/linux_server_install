
make  TARGET=linux3100 CPU=x86_64  PREFIX=/usr/local/haprpxy  #编译  uname -r #查看系统内核版本号
make install PREFIX=/usr/local/haproxy  #安装

#参数说明：
#TARGET=linux3100
#使用uname -r查看内核，如：2.6.18-371.el5，此时该参数就为linux26
#kernel 大于2.6.28的用：TARGET=linux2628
#CPU=x86_64   #使用uname -r查看系统信息，如x86_64 x86_64 x86_64 GNU/Linux，此时该参数就为x86_64
#PREFIX=/usr/local/haprpxy   #/usr/local/haprpxy为haprpxy安装路径

groupadd haproxy #添加haproxy组

useradd -g haproxy haproxy -s /bin/false #创建nginx运行账户haproxy并加入到haproxy组，不允许haproxy用户直接登录系统

mkdir -p  /usr/local/haproxy/conf  #创建配置文件目录

mkdir -p /etc/haproxy  #创建配置文件目录

touch  /usr/local/haproxy/conf/haproxy.cfg  #创建配置文件

ln -s  /usr/local/haproxy/conf/haproxy.cfg   /etc/haproxy/haproxy.cfg  #添加配置文件软连接

cp -r  /root/haproxy-1.7.0/examples/errorfiles /usr/local/haproxy/errorfiles  #拷贝错误页面

ln -s  /usr/local/haproxy/errorfiles  /etc/haproxy/errorfiles  #添加软连接

mkdir -p  /usr/local/haproxy/log  #创建日志文件目录

touch  /usr/local/haproxy/log/haproxy.log  #创建日志文件

ln -s  /usr/local/haproxy/log/haproxy.log  /var/log/haproxy.log  #添加软连接

cp /root/haproxy-1.7.0/examples/haproxy.init  /etc/rc.d/init.d/haproxy  #拷贝开机启动文件

chmod +x  /etc/rc.d/init.d/haproxy  #添加脚本执行权限

chkconfig haproxy on  #设置开机启动

ln -s  /usr/local/haproxy/sbin/haproxy  /usr/sbin  #添加软连接


cp  /usr/local/haproxy/conf/haproxy.cfg   /usr/local/haproxy/conf/haproxy.cfg-bak

vi  /usr/local/haproxy/conf/haproxy.cfg 
#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    log    127.0.0.1 local2          ###[err warning info debug] 
    chroot  /usr/local/haproxy
    pidfile  /var/run/haproxy.pid   ###haproxy的pid存放路径,启动进程的用户必须有权限访问此文件 
    maxconn  4000                   ###最大连接数，默认4000
    user   haproxy
    group   haproxy
    daemon                          ###创建1个进程进入deamon模式运行。此参数要求将运行模式设置为"daemon"
 
#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will 
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode   http             ###默认的模式mode { tcp|http|health }，tcp是4层，http是7层，health只会返回OK
    log    global           ###采用全局定义的日志
    option  dontlognull     ###不记录健康检查的日志信息
    option  httpclose       ###每次请求完毕后主动关闭http通道 
    option  httplog         ###日志类别http日志格式 
    option  forwardfor      ###如果后端服务器需要获得客户端真实ip需要配置的参数，可以从Http Header中获得客户端ip  
    option  redispatch      ###serverId对应的服务器挂掉后,强制定向到其他健康的服务器
    timeout connect 10000   #default 10 second timeout if a backend is not found
    timeout client 300000   ###客户端连接超时
    timeout server 300000   ###服务器连接超时
    maxconn     60000       ###最大连接数
    retries     3           ###3次连接失败就认为服务不可用，也可以通过后面设置 
####################################################################
listen stats
        bind 0.0.0.0:1080           #监听端口  
        stats refresh 30s           #统计页面自动刷新时间  
        stats uri /stats            #统计页面url  
        stats realm Haproxy Manager #统计页面密码框上提示文本  
        stats auth admin:admin      #统计页面用户名和密码设置  
        #stats hide-version         #隐藏统计页面上HAProxy的版本信息
#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
frontend main
    bind 0.0.0.0:80
    acl url_static path_beg    -i /static /images /javascript /stylesheets
    acl url_static path_end    -i .jpg .gif .png .css .js
 
    use_backend static if url_static     ###满足策略要求，则响应策略定义的backend页面
    default_backend   dynamic            ###不满足则响应backend的默认页面
 
#---------------------------------------------------------------------
# static backend for serving up images, stylesheets and such
#---------------------------------------------------------------------
 
backend static
    balance     roundrobin                 ###负载均衡模式轮询
    server      static 127.0.0.1:80 check ###后端服务器定义
     
backend dynamic
    balance    roundrobin
    server         websrv1 10.252.97.106:80 check maxconn 2000
    server         websrv2 10.117.8.20:80 check maxconn 2000
 
#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
#errorloc  503  http://www.osyunwei.com/404.html
errorfile 403 /etc/haproxy/errorfiles/403.http
errorfile 500 /etc/haproxy/errorfiles/500.http
errorfile 502 /etc/haproxy/errorfiles/502.http
errorfile 503 /etc/haproxy/errorfiles/503.http
errorfile 504 /etc/haproxy/errorfiles/504.http  
  
service haproxy start #启动

service haproxy stop  #关闭

service haproxy restart  #重启

vi  /etc/syslog.conf 
local0.*          /var/log/haproxy.log

local3.*          /var/log/haproxy.log

vi  /etc/sysconfig/syslog  
SYSLOGD_OPTIONS="-r -m 0"   #接收远程服务器日志


