1.nginx自带的ngx_http_upstream_module模块严格说来是没有健康检查的功能的，它只能做到在请求的节点故障时将请求再次转发到另外的节点
因此如果后端有不健康节点，负载均衡器依然会先把该请求转发给该不健康节点，然后失败后再转发给别的节点，这样就会增加一次处理的时间，并且如果不健康节点不是完全挂掉，只是半死不活响应慢时，会严重拖慢第一次请求的时间
所以不推荐使用该方式
 
2.推荐使用淘宝的nginx_upstream_check_module模块
该模块可以用来检测后端realserver的健康状态，如果后端realserver 不可用，则所有的请求就不会转发到该节点上，最重要的是该模块还支持7层检查
该模块需要打补丁，详细情况参考最下面的内容：
 
3.关于健康检查的时间等相关参数设置，一般使用默认的设置即能满足需求，4层健康检查推荐使用如下：
check interval=3000 rise=2 fall=3 timeout=3000
3秒检查一次，连续2次成功则认为节点存活，失败3次则认为节点故障，每次检查超时时间为3秒
还可以使用7层检查，详细情况参考最下面的内容
 
4.nginx打补丁使用淘宝的nginx_upstream_check_module
补丁地址：https://github.com/yaoweibin/nginx_upstream_check_module
 
补丁列表备选:
https://github.com/yaoweibin/nginx_upstream_check_module
 
给nginx源码打补丁
# cd nginx-1.10.1 #进入nginx的源码目录
# patch -p0 < /root/nginx_upstream_check_module/nginx_upstream_check_module-master/check_1.9.2+.patch
# ./configure --add-module=/root/nginx_upstream_check_module/nginx_upstream_check_module-master/ #编译参数需要和之前的一样，只是增加了nginx_upstream_check_module
#make
# mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx-1.6.0.bak
# cp objs/nginx /usr/local/nginx/sbin/ #将新编译的nginx二进制程序文件替换掉原先的
 
修改nginx配置
在nginx.conf配置文件里面的upstream加入健康检查，如下：
upstream name {
server 192.168.0.21:80;
server 192.168.0.22:80;
check interval=3000 rise=2 fall=3 timeout=1000 type=http;
check_http_send "HEAD /healthcheck/status HTTP/1.0\r\n\r\n";
check_http_expect_alive http_2xx http_3xx;
}
上面配置的意思是，对name这个负载均衡条目中的所有节点，每隔3秒检测一次，请求2次正常则标记realserver状态为up，如果检测3次都失败，则标记 realserver的状态为down，超时时间为1秒，检查为7层http检查，检查页面为/healthcheck/status，返回结果为2xx和3xx认为正常
