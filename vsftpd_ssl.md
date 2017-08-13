FTP over SSL (Implicit)隐式ssl与FTP over SSL (Explicit)显式ssl：
vsftp默认启动时用的是显式ssl，也可以配置启用隐式ssl，对应端口21(可修改成990)

显式ssl： 在与ftp服务器建立连接后，ftp客户端要以命令（"AUTH SSL" 或者 "AUTH TLS"）显式地告诉服务器端来初始化相应的安全连接。此时使用的是默认的ftp端口21。参考文档：RFC 2228

隐式ssl：当ftp客户端连接到服务器端时，服务器端自动建立安全连接。此时，客户端默认以990端口来安全连接服务器端，而服务器端端口可设置。

检查vsftp是否支持SSL
ldd /usr/sbin/vsftpd | grep libssl

openssl req -new -x509 -nodes -out vsftpd.pem -keyout vsftpd.pem -days 3650
生成证书

vsftpd.conf添加配置:
# For test, I use vsftp default certificate file
rsa_cert_file=/etc/vsftpd/.sslkey/vsftpd.pem
# Turn on SSL
ssl_enable=YES
ssl_sslv2=YES
ssl_sslv3=YES
ssl_tlsv1=YES
force_local_logins_ssl=YES #强制匿名用户使用加密登陆和数据传输，若为NO时，可选加密或者不加密
force_local_data_ssl=YES
# Disable SSL session reuse(required by WinSCP)
require_ssl_reuse=NO
# Select which SSL ciphers vsftpd will allow for encrypted SSL connections
# (required by FileZilla)
ssl_ciphers=HIGH


默认不启用隐式ssl功能，相应的服务器端隐式ssl默认端口是21（很多客户端隐式ssl连接时，设置的默认端口为990，因此如果服务器的不自定义成和客户端一致的话，会导致连接失败！）。如果启用了隐式ssl，那么ftp客户端也必须以隐式ssl的方式连接到21/990端口，ftp客户端的不加密连接、显式ssl连接都会超时。所以不建议开启该设置！


winscp连接:
下载证书,指定加密协议,高级中指定证书
