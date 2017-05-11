https://certbot.eff.org/


为了通过 Let’s Encrypt 的验证，验证 linuxstory.org 这个域名是属于我的管理之下。
location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root     /home/wwwroot/linuxstory.org/;
}
 
location = /.well-known/acme-challenge/ {
   return 404;
}



certbot （实际上是 certbot-auto ） 有两种方式生成证书：

standalone 方式： certbot 会自己运行一个 web server 来进行验证。如果我们自己的服务器上已经有 web server 正在运行 （比如 Nginx 或 Apache ），用 standalone 方式的话需要先关掉它，以免冲突。
webroot 方式： certbot 会利用既有的 web server，在其 web root目录下创建隐藏文件， Let’s Encrypt 服务端会通过域名来访问这些隐藏文件，以确认你的确拥有对应域名的控制权。

