nginx：
```
http://nginx.org/en/docs/http/ngx_http_sub_module.html

./configure --with-http_sub_module

在需要的路径加上下面的配置:
vi /usr/local/nginx/conf/nginx.conf
   location / {
        .....
            sub_filter '</head>'  '</head><script language="javascript" src="uem.js"></script>';
            sub_filter_once on;
   }

cd /usr/local/nginx/html/
echo 'alert("1")' > uem.js

```

apache:
```
https://httpd.apache.org/docs/2.4/mod/mod_substitute.html

grep "mod_substitute.so" /etc/httpd/conf.modules.d/*
grep "mod_filter.so" /etc/httpd/conf.modules.d/*
假如注释的情况下，就把注释去掉就好。

echo 'alert("1")' > /var/www/html/uem.js

vi /etc/httpd/conf/httpd.conf
在需要的路径加上下面的配置:
<Location "/">
   AddOutputFilterByType SUBSTITUTE text/html
   Substitute "s|<head>|<head><script src='uem.js'></script>|in"
</Location>
```
