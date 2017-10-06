```
https://github.com/mpx/lua-cjson/releases
https://www.kyne.com.au/~mark/software/lua-cjson-manual.html

优点：快，支持utf8，没有其他库依赖，MIT开源
缺点：不支持utf16及utf32，2012年之后就没更新了

wget https://github.com/mpx/lua-cjson/archive/2.1.0.tar.gz
tar zxvf 2.1.0.tar.gz
 
find / -name lua.h
修改Makefile文件：
打开后发现LUA_INCLUDE_DIR =   $(PREFIX)/include设置有问题
修改LUA_INCLUDE_DIR =   /usr/include/lua5.1  find的结果路径

make
make install

/usr/local/lib/lua/5.1/cjson.so


lua代码：
package.path = '/usr/local/lib/lua/?.lua;'
package.cpath = '/usr/local/lib/lua/5.1/?.so;'

nginx配置中修改：
include lua.conf

lua.conf:
lua_package_path  '/usr/local/lib/lua/resty/?.lua;;';
lua_package_cpath  '/usr/local/lib/lua/?.so;;';
```

lua test.lua
```lua
package.cpath = '/usr/local/lib/lua/5.1/?.so;'  
  
--json  
local cjson = require "cjson"  
  
local json_data = '{"name":"tom", "age":"10"}'  
  
local unjson = cjson.decode(json_data)  
print(unjson["name"])  
  
local json_data2 = cjson.encode(unjson)  
print(json_data2)
```
