```
mkdir /usr/local/src
cd /usr/local/src

wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz
tar zxvf LuaJIT-2.0.5.tar.gz
cd LuaJIT-2.0.5
make PREFIX=/usr/local/luajit
make install PREFIX=/usr/local/luajit

cd /usr/local/src
wget https://github.com/openresty/lua-nginx-module/archive/v0.10.10.tar.gz
tar -xzvf v0.10.10.tar.gz

cd /usr/local/src
wget http://nginx.org/download/nginx-1.13.5.tar.gz
export LUAJIT_LIB=/usr/local/luajit/lib
export LUAJIT_INC=/usr/local/luajit/include/luajit-2.0
tar zxvf nginx-1.13.5.tar.gz
cd nginx-1.13.5
./configure --prefix=/usr/local/nginx-1.13.5 --add-module=/usr/local/src/lua-nginx-module-0.10.10
make -j2
make install


ln -s /usr/local/luajit/lib/libluajit-5.1.so.2 /lib64/libluajit-5.1.so.2

vi conf/nginx.conf
location ~* ^/2328(/.*) {
      default_type 'text/plain';
      content_by_lua 'ngx.say("hello, ttlsa lua")';
}

location /lua-version {
            content_by_lua '
                if jit then
                    ngx.say(jit.version)
                else
                    ngx.say(_VERSION)
                end
            ';
}
         
location /user-aciton { // 导入 lua脚本 的方式
               default_type 'text/plain';
               content_by_lua_file /Users/admin/Developer/workspace/lua/post_user_action.lua;
}
          
sbin/nginx

curl http://10.1.50.201/2328/ 
hello, ttlsa lua

```
