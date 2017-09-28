```
ngx_devel_kit:
https://github.com/simpl/ngx_devel_kit

NDK（nginx development kit）模块是一个拓展nginx服务器核心功能的模块，第三方模块开发可以基于它来快速实现。

NDK提供函数和宏处理一些基本任务，减轻第三方模块开发的代码量。

开发者如果要依赖这个模块做开发，需要将这个模块一并参与nginx编译，同时需要在自己的模块配置中声明所需要使用的特性。


--add-module=/usr/local/src/ngx_devel_kit-0.2.19

```
